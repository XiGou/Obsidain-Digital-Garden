```bash
BUILDER_IMAGE_NAME = registry.smtx.io/zbs/zbs-buildtime

RUNTIME_IMAGE_NAME = registry.smtx.io/zbs/zbs-runtime

TAG = latest

PROJECT_ROOT := $(shell dirname $(shell dirname $(CURDIR)))

  

.PHONY: all build clean

  

all: build

  

# Ubuntu 22.04 amd64

build_image_amd64_ubuntu22.04:

docker build -f ./ubuntu/Dokerfile.amd64.ubuntu2204 -t $(BUILDER_IMAGE_NAME):jammy-amd64 .

  

test_image_amd64_ubuntu22.04:

docker run --rm -it --privileged -u root \

-v $(PROJECT_ROOT)/debs/:/home/builder \

-v $(PROJECT_ROOT):/home/builder/zbs \

-v /tmp:/tmp -w /home/builder/zbs \

$(BUILDER_IMAGE_NAME):jammy_amd64_22.04 bash

  

build_deb_amd64_ubuntu22.04: build_image_amd64_ubuntu22.04

docker run --rm --privileged -u root -v $(pwd)/debs/:/home/builder \

-v $(PROJECT_ROOT):/home/builder/zbs \

-v /tmp:/tmp \

-w /home/builder/zbs \

registry.smtx.io/zbs/zbs-buildtime:jammy-amd64 \

bash -c "script/gen_deb_changelog.py | tee debian/changelog; dpkg-buildpackage -us -uc -b"

  

# Ubuntu 22.04 arm64

build_image_arm64_ubuntu22.04:

docker build -f ./ubuntu/Dokerfile.arm64.ubuntu2204 -t $(BUILDER_IMAGE_NAME):jammy-arm64 .

  

test_image_arm64_ubuntu22.04:

docker run --rm -it --privileged \

-u root \

-v $(PROJECT_ROOT)/debs/:/home/builder \

-v $(PROJECT_ROOT):/home/builder/zbs \

-v /tmp:/tmp \

-w /home/builder/zbs \

$(BUILDER_IMAGE_NAME):jammy-arm64 \

bash

  

build_deb_arm64_ubuntu22.04: build_image_arm64_ubuntu22.04

docker run --rm --privileged -u root -v $(pwd)/debs/:/home/builder \

-v $(PROJECT_ROOT):/home/builder/zbs \

-v /tmp:/tmp \

-w /home/builder/zbs \

registry.smtx.io/zbs/zbs-buildtime:jammy-arm64 \

bash -c "script/gen_deb_changelog.py | tee debian/changelog; dpkg-buildpackage -us -uc -b"

  

# oe 2203 amd64

build_image_amd64_oe2203:

docker build -f ./openeuler/Dockerfile.oe2203 -t $(BUILDER_IMAGE_NAME):oe2203-amd64 .

  

test_image_amd64_oe2203:

docker run --rm -it --privileged \

-u root \

-v $(PROJECT_ROOT):/home/builder/zbs \

-w /home/builder/zbs \

$(BUILDER_IMAGE_NAME):oe2203-amd64 bash

  

build_rpm_amd64_oe2203:

docker run --rm -it --privileged \

-u root \

-v $(PROJECT_ROOT):/home/builder/zbs \

-w /home/builder/zbs \

$(BUILDER_IMAGE_NAME):oe2203-amd64 \

bash -c "rm -rf build && mkdir build && cd build && cmake -GNinja .. && ninja rpm"

  
  

# oe 2203 arm64

build_image_arm64_oe2203:

docker build -f ./openeuler/Dockerfile.arm64.oe2203 -t $(BUILDER_IMAGE_NAME):oe2203-arm64 .

  

test_image_arm64_oe2203:

docker run --rm -it --privileged \

-u root \

-v $(PROJECT_ROOT):/home/builder/zbs \

-w /home/builder/zbs \

$(BUILDER_IMAGE_NAME):oe2203-arm64 bash

  

build_rpm_arm64_oe2203:

docker run --rm -it --privileged \

-u root \

-v $(PROJECT_ROOT):/home/builder/zbs \

-w /home/builder/zbs \

$(BUILDER_IMAGE_NAME):oe2203-arm64 \

bash -c "rm -rf build && mkdir build && cd build && cmake -GNinja .. && ninja rpm"

  

###

  

build_image_amd64_ubuntu20.04:

docker build -f ./ubuntu/Dockerfile.ubuntu20.04 -t $(BUILDER_IMAGE_NAME):$(TAG)_20.04 .

  

test_image_amd64_ubuntu20.04:

docker run --rm -it --privileged -u root -v $(PROJECT_ROOT)/debs/:/home/builder -v $(PROJECT_ROOT):/home/builder/zbs -v /tmp:/tmp -w /home/builder/zbs $(BUILDER_IMAGE_NAME):$(TAG)_20.04 bash

  
  
  
  
  
  

build/runtime/el7/amd64:

docker build -f el7/Dockerfile.el7.amd64.runtime --tag ${RUNTIME_IMAGE_NAME}:el7-amd64 .

  

test/runtime/el7/amd64:

docker run --rm -it -v ${PWD}/../..:/zbs ${RUNTIME_IMAGE_NAME}:el7-amd64 bash

  

build/runtime/el7/arm64:

docker build -f el7/Dockerfile.el7.arm64.runtime --tag ${RUNTIME_IMAGE_NAME}:el7-arm64 .

  

build_image_amd64_oe2003:

docker build -f ./openeuler/Dockerfile.oe2003 -t $(BUILDER_IMAGE_NAME):$(TAG)_oe2003 .

  
  

clean:

docker rmi $(BUILDER_IMAGE_NAME):$(TAG)
```