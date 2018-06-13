FROM debian:stretch-slim

RUN USE_OPEN_BLAS="true" ; \
	apt-get update ; \
	apt-get install -y build-essential git libatlas-base-dev \
		libopencv-dev python-opencv \
	    libcurl4-openssl-dev libgtest-dev cmake wget unzip ; \
	if [ "${USE_OPEN_BLAS" = "true" ]; then \
		apt-get install -y libopenblas-dev liblapack-dev ; \
	fi ; \
	cd /usr/src/gtest ; \
	cmake CMakeLists.txt ; \
	make ; \
	cp *.a /usr/lib ; \
	git clone --recursive https://github.com/dmlc/mxnet ; \
	cd mxnet ; \
	MXNET_COMPILE_FLAGS="" ; \
	if [ "${USE_OPEN_BLAS}" = "true" ]; then \
		MXNET_COMPILE_FLAGS="USE_OPENCV=1 USE_BLAS=openblas" ; \
	fi ; \
    make -j$(nproc) "${MXNET_COMPILE_FLAGS}"; \
    rm -r build ; \
    apt-get update ; \
    cd /tmp ; \
    apt-get install -y python-dev ; \
    wget https://bootstrap.pypa.io/get-pip.py ; \
    python2 get-pip.py ; \
	pip2 install nose pylint numpy nose-timer requests Pillow ; \
	apt-get install -y python3-dev ; \
    python3 get-pip.py ; \
	pip3 install nose pylint numpy nose-timer requests Pillow ; \
	apt-get autoremove

ENV PYTHONPATH=/mxnet/python

# To remove the following error caused by opencv
#    libdc1394 error: Failed to initialize libdc1394"
CMD sh -c 'ln -s /dev/null /dev/raw1394'; bash
