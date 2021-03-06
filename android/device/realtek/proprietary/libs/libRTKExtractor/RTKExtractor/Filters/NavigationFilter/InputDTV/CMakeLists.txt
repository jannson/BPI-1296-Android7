INCLUDE(../../../../include/config.txt)

SET(LIBNAME InputDTV)
SET(LIBRARY_OUTPUT_PATH ../../../../lib/)

SET(rm " /bin/rm -f")
SET(CC " ${MYCC}")
SET(LINK " -=LINK=-")
SET(DEFS )
SET(AR " ${MYAR} cq")
SET(RANLIB " ${MYRANLIB}")

SET(INCLUDES ${INCLUDES} " ${CMAKE_CURRENT_SOURCE_DIR}/../../../Include")
SET(INCLUDES ${INCLUDES} " ${CMAKE_CURRENT_SOURCE_DIR}/../../../../../common/IPC/include")
SET(INCLUDES  ${INCLUDES} " ${CMAKE_CURRENT_SOURCE_DIR}/../../../../../common/IPC/generate/include/system")

SET(DEFINES ${DEFINES} " ${DEFS} -DSYS_UNIX=1")
SET(DEFINES ${DEFINES} " -DERROR_CONCEALMENT_LEVEL=${ERROR_CONCEALMENT_LEVEL}")

IF(RESET_DTV_FILE_PLAYBACK_RESYNC_THRESHOLD STREQUAL YES)
SET(DEFINES ${DEFINES} " -DDTV_FILE_PLAYBACK_RESYNC_THRESHOLD=${DTV_FILE_PLAYBACK_RESYNC_THRESHOLD}")
ELSE(RESET_DTV_FILE_PLAYBACK_RESYNC_THRESHOLD STREQUAL YES)
SET(DEFINES ${DEFINES} " -DDTV_FILE_PLAYBACK_RESYNC_THRESHOLD=962560")
ENDIF(RESET_DTV_FILE_PLAYBACK_RESYNC_THRESHOLD STREQUAL YES)

SET(CFLAGS " ${DEFINES} ${COMPILE_OPT}")
SET(LIBS )

IF(__NAV_USE_DLL_PLUGIN__ STREQUAL YES)
SET(LIBNAME ../../../../lib/InputDTV.so)
SET(CFLAGS ${CFLAGS} " -fPIC -D__NAV_USE_DLL_PLUGIN__")
SET(LIB_DIRS $(LIB_DIRS) " ${CMAKE_CURRENT_SOURCE_DIR}/../../../../lib")
SET(LIBS $(LIBS) " OSAL")
ELSE(__NAV_USE_DLL_PLUGIN__ STREQUAL YES)
#SET(LIBNAME ../../../../lib/libInputDTV.a)
ENDIF(__NAV_USE_DLL_PLUGIN__ STREQUAL YES)

SET(SRCS CInputDTV.cpp CDtvFileReader.cpp CDtvFileWriter.cpp CDtvBitRateEst.cpp CDtvContextInfo.cpp CDtvFileEditor.cpp)
SET(OBJS CInputDTV.o CDtvFileReader.o CDtvFileWriter.o CDtvBitRateEst.o CDtvContextInfo.o CDtvFileEditor.o)
IF(__NAV_USE_DLL_PLUGIN__ STREQUAL YES)
ELSE(__NAV_USE_DLL_PLUGIN__ STREQUAL YES)
ENDIF(__NAV_USE_DLL_PLUGIN__ STREQUAL YES)

INCLUDE_DIRECTORIES(${INCLUDES})
ADD_DEFINITIONS(${CFLAGS} ${DEBUGFLAG} ${WARNING})
ADD_LIBRARY(${LIBNAME} ${SRCS})
string(REGEX REPLACE  ";" " " APCFG_DVR  ${APCFG_DVR})
string(REGEX REPLACE  ";" " " CHIP_MODEL_DEFS ${CHIP_MODEL_DEFS})
string(REGEX REPLACE  ";" " " APCFG_DEFS ${APCFG_DEFS})
#string(REGEX REPLACE  ";" " " BUILD_OPTIONS ${BUILD_OPTIONS})
string(REGEX REPLACE  ";" " " DEFINES ${DEFINES})
string(REGEX REPLACE  ";" " " APCFG_FLAGS ${APCFG_FLAGS})
