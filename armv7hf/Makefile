NAME := bellx2/acap_opencv:3.4.2-armv7hf

image:
	docker build -t $(NAME) .

scan:
	docker scan $(NAME)

push:
	docker push $(NAME)

run:
	docker run -it $(NAME)

clean:
	docker rmi -f $(NAME)

