#include ../public/Makefile.def
MODULE_NAME=test_thrift

BINDIR=./output
TARGET=test/test_server
TARGET1=test/test_client
INCLUDE_PATH=./include
LIBRARY_PATH=./lib
SHARED=shared

INC=-I$(INCLUDE_PATH) -I./
LIB=-L$(LIBRARY_PATH) -lrt -lcrypto -ldl -lz $(LIBRARY_PATH)/*.o -lboost_thread -lboost_regex -lboost_system -lpthread -lthrift -lthriftnb -levent

#DC=g++ -g -Wall -DBUILD_TIME="$(BUILD_TIME)"  -DPROJECT_VERSION="$(PROJECT_BUILD_VERSION)" -DMODULE_VERSION="$(MODULE_BUILD_VERSION)" -DTAG_VERSION="$(TAG_LAST)" -o $@ -c $<
#DT=g++ -g -Wall -DBUILD_TIME="$(BUILD_TIME)"  -DPROJECT_VERSION="$(PROJECT_BUILD_VERSION)" -DMODULE_VERSION="$(MODULE_BUILD_VERSION)" -DTAG_VERSION="$(TAG_LAST)" -o $@ $^

DC=g++ -std=c++11 -g -Wall -o $@ -c $<
DT=g++ -std=c++11 -g -Wall -o $@ $^

AR = ar r
RANLIB = ranlib

release:sub_dir static_lib clrtarget $(TARGET) $(TARGET1) 
	cp $(TARGET) $(BINDIR)
	cp $(TARGET1) $(BINDIR)

sub_dir: dir
#   -@mkdir include
#   -@mkdir lib
	@for d in $(SHARED) ; do \
	make -C $$d ; \
	echo "" ;\
	done

dir:
	@if [ ! -d $(INCLUDE_PATH) ]; then mkdir $(INCLUDE_PATH); fi
	@if [ ! -d $(LIBRARY_PATH) ]; then mkdir $(LIBRARY_PATH); fi

static_lib: 
	$(AR) ./lib/lib$(MODULE_NAME).a ./lib/*.o
	$(RANLIB) ./lib/lib$(MODULE_NAME).a

clrtarget:
	-@rm -rf $(TARGET)
	-@rm -rf $(TARGET1)

OBJS_TEST_SERVER = test/server.o 
test/test_server:$(OBJS_TEST_SERVER) 
	$(DT) $(LIB) 
	@echo ""

OBJS_TEST_CLIENT = test/client.o 
test/test_client:$(OBJS_TEST_CLIENT) 
	$(DT) $(LIB) 
	@echo ""

%.o:%.cpp 
	$(DC) $(INC)

%.o:%.cc 
	$(DC) $(INC)

%.o:%.c 
	$(DC) $(INC)

%.o:%.cpp %.h
	$(DC) $(INC)

%.o:%.cc %.h
	$(DC) $(INC)

%.o:%.c %.h
	$(DC) $(INC)

clean: dir
	@for d in $(SHARED) ; do \
	make clean -C $$d ; \
	echo "" ;\
	done
	-@rm -rf $(INCLUDE_PATH)/*.h
	-@rm -rf $(INCLUDE_PATH)/json
	-@rm -rf $(LIBRARY_PATH)/*.o $(LIBRARY_PATH)/*.a
	-@rm -rf  *.o
	-@rm -rf  test/*.o
	-@rm -rf $(TARGET)
	-@rm -rf $(TARGET1)
	-@rm -rf output/*


