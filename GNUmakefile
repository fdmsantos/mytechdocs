build:
	docker build -t mytechdocs .

serve:
	docker run --rm -it -p 8000:8000 -v ${PWD}:/docs mytechdocs

run: build
	$(MAKE) serve