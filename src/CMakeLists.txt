
INCLUDE(GNUInstallDirs)

MACRO(ARDOP2OFDM_APPEND_SRCS)
	LIST(APPEND ardop2ofdm_srcs ${ARGV})
ENDMACRO(ARDOP2OFDM_APPEND_SRCS)

ARDOP2OFDM_APPEND_SRCS(
		ofdm.c
		LinSerial.c
		KISSModule.c
		pktARDOP.c
		pktSession.c
		BusyDetect.c
		i2cDisplay.c
		ALSASound.c
		ARDOPC.c
		ardopSampleArrays.c
		ARQ.c
		FFT.c
		FEC.c
		HostInterface.c
		Modulate.c
		rs.c
		berlekamp.c
		galois.c
		SoundInput.c
		TCPHostInterface.c
		SCSHostInterface.c
)


########################################################################
# Setup libraries used in executables and build executable
########################################################################

ADD_EXECUTABLE(ardop2ofdm ${ardop2ofdm_srcs})

SET(INSTALL_TARGETS ardop2ofdm)

TARGET_LINK_LIBRARIES(ardop2ofdm ${CMAKE_THREAD_LIBS_INIT})


FIND_LIBRARY(LIBRT rt)

IF (LIBRT)
	MESSAGE("Found librt: ${LIBRT}")
	TARGET_LINK_LIBRARIES(ardop2ofdm ${LIBRT})
ELSE ()
	MESSAGE(FATAL_ERROR "librt not found")
ENDIF()

TARGET_LINK_LIBRARIES(ardop2ofdm m)

IF (ALSA_FOUND)
	MESSAGE("Found libasound: ${ALSA_LIBRARY}")
	TARGET_LINK_LIBRARIES(ardop2ofdm ${ALSA_LIBRARIES})
ELSE ()
	MESSAGE(FATAL_ERROR "libasound not found")
ENDIF ()


########################################################################
# Install built executable and other files
########################################################################

INSTALL(TARGETS ${INSTALL_TARGETS}
		LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} # .so/.dylib file
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} # .lib file
		RUNTIME DESTINATION bin              # .dll file
)
