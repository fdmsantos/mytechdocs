# DBT

[Slides](files/Complete+dbt+Bootcamp+slides.pdf)

[Github](https://github.com/nordquant/complete-dbt-bootcamp-zero-to-hero)

## Slow Dimension Changes

### Type 1 and Type 2 in Same Table

[Solution](https://servian.dev/modelling-type-1-2-slowly-changing-dimensions-with-dbt-1b80078f290a)

This solution allows catch SCD Type 1 for some columns and SCD Type 2 for other columns in same table

#### Capturing Type 2 Changes

```jinja2
{% snapshot t2_listing %}

{{
  config(
    target_schema = 't2_schema',
    unique_key = 'listid',
    strategy = 'check',
    check_cols = ['sellerid','eventid','dateid']
    )
}}

{%- set t1_cols = ['numtickets','priceperticket','totalprice','listtime'] -%}

select listid::varchar || '-' || to_char(convert_timezone('Australia/Melbourne',sysdate::timestamp),'YYYYMMDDHH24MISS') as listing_key,
        *,
        {{ dbt_utils.surrogate_key(t1_cols) }} as t1_key
from {{ source('tickit', 'listing') }}

{% endsnapshot %}
```

#### Capturing Type 1 Changes

```jinja2
{% macro dim_type_1_cols(t1_cols, dim_name, inc_prefix, t2_prefix, source_prefix) %}
    {% for col in t1_cols -%}
        {% if is_incremental() -%}
        {#- Incremental Run case -#}
            case when ({{inc_prefix}}.dbt_valid_to is null  
                        and {{inc_prefix}}.t1_key != {{source_prefix}}.t1_key) then {{source_prefix}}.{{col}}
                when ({{inc_prefix}}.{{ dim_name }}_key is not null) then {{inc_prefix}}.{{col}}
                else {{t2_prefix}}.{{col}} end as {{col}} {%- if not loop.last -%},{%- endif -%}
        {%- else %} 
        {#- Initial Run Case -#}
            case when ({{t2_prefix}}.dbt_valid_to is null 
                        and {{t2_prefix}}.t1_key != {{source_prefix}}.t1_key ) then {{source_prefix}}.{{col}}
                else {{t2_prefix}}.{{col}} end as {{col}} {%- if not loop.last -%},{%- endif -%}
        {%- endif %}
    {% endfor -%}
{% endmacro %}
```

#### Dimension Metadata (Timestamps)

```jinja2
{% macro dim_update_timestamp(dim_name, inc_prefix, t2_prefix, source_prefix) %}
    {% if is_incremental() -%}
    {#- Incremental Run Case -#}
        case when ({{t2_prefix}}.dbt_valid_to is null 
                    and {{inc_prefix}}.t1_key != {{source_prefix}}.t1_key) then convert_timezone('Australia/Melbourne',sysdate::timestamp)
             when ({{inc_prefix}}.{{dim_name}}_key is not null) then {{inc_prefix}}.dbt_updated_at
             else convert_timezone('Australia/Melbourne',{{t2_prefix}}.dbt_updated_at::timestamp) end as dbt_updated_at
    {%- else %}
    {#- Initial Run Case -#}
        case when ({{t2_prefix}}.dbt_valid_to is null 
                    and {{t2_prefix}}.t1_key != {{source_prefix}}.t1_key ) then convert_timezone('Australia/Melbourne',sysdate::timestamp)
             else convert_timezone('Australia/Melbourne',{{t2_prefix}}.dbt_updated_at::timestamp) end as dbt_updated_at
    {%- endif %}
{% endmacro %}
```

#### Model

```jinja2
{{
  config(
    materialized = 'incremental',
    unique_key = 'listing_key',
    )
}}

{#- Define the different Type column names we need -#}
{%- set t1_cols = ['numtickets','priceperticket','totalprice','listtime'] -%}
{%- set t2_cols = ['listid','sellerid','eventid','dateid'] -%}

{#- Views to compute the surrogate key for Type 1 changes -#}
with v_listing as
(
    select *,
           {{ dbt_utils.surrogate_key(t1_cols) }} as t1_key
    from {{ source('tickit', 'listing') }}
)
{#- Only creates the view for incremental runs -#}
{% if is_incremental() -%}
, v_this as
(
    select *,
           {{ dbt_utils.surrogate_key(t1_cols) }} as t1_key
    from {{ this }}
)
{%- endif %}

select t2.listing_key,
    {#- Reflect Type 2 changes from snapshot -#}
    {%- for col in t2_cols -%}
      t2.{{col}} as {{col}},
    {% endfor -%}
    {#- Handle Type 1 changes -#}
      {{ dim_type_1_cols(t1_cols,'listing','d','t2','s') }},
    {#- Updating updated_at timestamp if there is a type 1 change  -#}
      {{ dim_update_timestamp('listing','d','t2','s') }},
    {#- Converting dbt timestamps to AEST time as dbt default is UTC -#}
      convert_timezone('Australia/Melbourne',t2.dbt_valid_from::timestamp) as dbt_valid_from,
      convert_timezone('Australia/Melbourne',t2.dbt_valid_to::timestamp) as dbt_valid_to
from {{ ref('t2_listing') }} t2
    left join v_listing s
        on t2.listid = s.listid
    {# Checks previous records on it self for incremental runs -#}
    {% if is_incremental() -%}
    left join v_this d
        on t2.listing_key = d.listing_key
    {%- endif %}
```