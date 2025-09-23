# the base image to use
FROM alpine
# creating a directory to run the application in
VOLUME /mydata
# set the working directory
WORKDIR /mydata
# copy the file from the current Director to inside the image
COPY countdown .
# set the initial command, which will run the countdown script
RUN chmod +x countdown
# Make the countdown script executable
ENTRYPOINT ["./countdown"]
# by default this will run for 1 minute, if you want to run for more minutes pass argument to docker run command
# docker run myapp 3  - to run for 3 minutes
CMD ["1"]