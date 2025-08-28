# StrongSwan

## Swanctl IPSEC VPN IKEv1 + PSK + XAUTH

```json
connections {
	<vpn_name> {
		version = 1
		aggressive = yes
		pull = yes
        dpd_delay = 10s
		dpd_timeout = 30s
		vips = 0.0.0.0

		# Remote Gateway
		remote_addrs = <REMOTE_GATEWAY_ADDRESS>
		remote-psk {
			auth = psk
		}

		# Local
		local-psk {
			round = 0
			auth = psk
			id = <LOCAL_ID>
		}

		local-xauth {
			round = 1
			auth = xauth
            xauth_id = <XAUTH_USER>
		}

		# IKE Phase 1
		proposals = aes128-sha256-modp2048, aes256-sha256-modp2048
		
		# IKE Phase 2
		children {
			cvt {
				esp_proposals = aes128-sha256-modp2048, aes256-sha256-modp2048
                mode = tunnel
                local_ts = dynamic
                remote_ts = 10.0.0.0/8 # Routes to receive
				dpd_action = clear
			}
		}
	}
}


secrets {
	ike-<vpn_name> {
		id = <LOCAL_ID>
		secret = "<PSK>"
	}

	xauth-<vpn_name> {
		id = <XAUTH_USER>
		secret = "<XAUTH_PASS> 
	}
}
```

## Usefully Links

[Phase 2 negotiation fails on FortiGate VPN: INFORMATIONAL_V1 request meets with DELETE for IKE_SA response](https://github.com/strongswan/strongswan/discussions/2343)