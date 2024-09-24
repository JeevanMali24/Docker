# Docker

Hardware vertulazation
- suppose we have a hardware which contain memory of 20gb and want to install any software which requirds any 500mb, but we uable to install software because the memory has been accoupied by windows 10gb and linux 10gb
- this is what happen in hardware vertulazation
- to over come the drow back of hardware vertulazation we can use os level vertulazation

Os vertulazation
- in os vertulazation we can reuse the memory resources but in case of hardware level vertulazation we can't use memory once it allocated.


# what is docker
- docker is a platform that helps developer to build, run and share application with containers.
- docker is an open soure containerization platform. It enables developers to package application into containers.
- it is use to manage the life cycle of the container
- i use the docker to build the images, write docker file, run docker images

# Docker life cycle
- in entair docker life cycle means writing a docker file building a docker image to creating a Docker container and pushing them to registry.
- Deap explaination->
  - In my case what i usually do i start with writing a docker file, let's say a development team has approch or there is requirement from the project to containerize an application so i will initially start with writing a dockerfile
  - In docker file i will write the set of instrusction that are required to run the application and once I fill that the docker file is complite, I will create image out of it that you cna create the doceker CLI
  - In this i can use a docekr build, which will essentially convert your docker file to Docker Image.
  - and after that you cna again use docker run command to execute your container so again you can use docker CLI to create a container, finally you can pull your image to external Registries like docker hub or query.io
                                                                  docekr file
                                                                      |
                                                                  docker Image
                                                                      |
                                                                  Run the docekr Image
                                                                      |
                                                                  Create the docekr Container
                                                                      |
                                                                  You can pull you image to external registries(docker hub, query.io)
  - users would create a docekrfile with a set of instructions or commaands taht defines a docker image.
  - for e.g - which basic image to choose.
            - what dependence should be installed for the application to run.
  - Docker image act as a set of instruction to build a docker conatiner. It can be commposed to a snapshot in vm.
