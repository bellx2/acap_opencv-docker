NAME := bellx2/acap_opencv:native-1.15-aarch64

image:
	docker build -t $(NAME) .

push:
	docker push $(NAME)

run:
	docker run -it $(NAME)

clean:
	docker rmi -f $(NAME)

