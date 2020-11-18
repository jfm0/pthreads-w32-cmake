CMAKE_MINIMUM_REQUIRED (VERSION 2.6 FATAL_ERROR)

PROJECT (pthreads-w32)

####################
# Pthreads version
####################
SET (PTHREADS_W32_VERSION_MAJOR 2)
SET (PTHREADS_W32_VERSION_MINOR 9)
SET (PTHREADS_W32_VERSION_PATCH 1)
SET (PTHREADS_W32_VERSION "${PTHREADS_W32_VERSION_MAJOR}.${PTHREADS_W32_VERSION_MINOR}")
SET (PTHREADS_W32_VERSION_FULL "${PTHREADS_W32_VERSION}.${PTHREADS_W32_VERSION_PATCH}")

##########################
# Pthreads configuration
##########################

# Standard CMake option for building libraries shared or static by default.
OPTION (BUILD_SHARED_LIBS "Build with shared libraries." OFF)

# Additional include directories
INCLUDE_DIRECTORIES (${CMAKE_CURRENT_SOURCE_DIR})

# Preprocessor definitions
set(PTHREAD_DEFINITIONS ${PTHREAD_DEFINITIONS} __CLEANUP_C)
IF (BUILD_SHARED_LIBS)
    set(PTHREAD_DEFINITIONS ${PTHREAD_DEFINITIONS} PTW32_BUILD)
ELSE (BUILD_SHARED_LIBS)
    set(PTHREAD_DEFINITIONS ${PTHREAD_DEFINITIONS} PTW32_STATIC_LIB)
ENDIF (BUILD_SHARED_LIBS)

set(PTHREAD_DEFINITIONS ${PTHREAD_DEFINITIONS} HAVE_STRUCT_TIMESPEC)

#message(WARNING "CMAKE_CL_64=${CMAKE_CL_64}")
SET (RESOURCE_COMPILE_DEFINITIONS "PTW32_RC_MSC")
IF (CMAKE_CL_64)
  SET (RESOURCE_COMPILE_DEFINITIONS "${RESOURCE_COMPILE_DEFINITIONS};PTW32_ARCHx64")
ELSE (CMAKE_CL_64)
  SET (RESOURCE_COMPILE_DEFINITIONS "${RESOURCE_COMPILE_DEFINITIONS};PTW32_ARCHx86")
ENDIF (CMAKE_CL_64)
SET_PROPERTY (SOURCE version.rc PROPERTY COMPILE_DEFINITIONS ${RESOURCE_COMPILE_DEFINITIONS})
set(PTHREAD_DEFINITIONS ${PTHREAD_DEFINITIONS} ${RESOURCE_COMPILE_DEFINITIONS})
message(WARNING "PTHREAD_DEFINITIONS=${PTHREAD_DEFINITIONS}")

SET(SOURCE_FILES
    implement.h
    #pthread.c
    pthread.h
    sched.h
    semaphore.h
    version.rc
    pthread_setspecific.c
    pthread_getspecific.c
    pthread_key_create.c
    pthread_key_delete.c
	pthread_attr_init.c
	pthread_attr_destroy.c
	pthread_attr_getdetachstate.c
	pthread_attr_setdetachstate.c
	pthread_attr_getstackaddr.c
	pthread_attr_setstackaddr.c
	pthread_attr_getstacksize.c
	pthread_attr_setstacksize.c
	pthread_attr_getscope.c
	pthread_attr_setscope.c
	pthread_attr_setschedpolicy.c
	pthread_attr_getschedpolicy.c
	pthread_attr_setschedparam.c
	pthread_attr_getschedparam.c
	pthread_attr_setinheritsched.c
	pthread_attr_getinheritsched.c
	pthread_barrier_init.c
	pthread_barrier_destroy.c
	pthread_barrier_wait.c
	pthread_barrierattr_init.c
	pthread_barrierattr_destroy.c
	pthread_barrierattr_setpshared.c
	pthread_barrierattr_getpshared.c
	pthread_setcancelstate.c
	pthread_setcanceltype.c
	pthread_testcancel.c
	pthread_cancel.c
	cleanup.c
	pthread_condattr_destroy.c
	pthread_condattr_getpshared.c
	pthread_condattr_init.c
	pthread_condattr_setpshared.c
	pthread_cond_destroy.c
	pthread_cond_init.c
	pthread_cond_signal.c
	pthread_cond_wait.c
	create.c
	dll.c
	autostatic.c
	errno.c
	pthread_exit.c
	fork.c
	global.c
	pthread_mutex_init.c
	pthread_mutex_destroy.c
	pthread_mutexattr_init.c
	pthread_mutexattr_destroy.c
	pthread_mutexattr_getpshared.c
	pthread_mutexattr_setpshared.c
	pthread_mutexattr_settype.c
	pthread_mutexattr_gettype.c
	pthread_mutexattr_setrobust.c
	pthread_mutexattr_getrobust.c
	pthread_mutex_lock.c
	pthread_mutex_timedlock.c
	pthread_mutex_unlock.c
	pthread_mutex_trylock.c
	pthread_mutex_consistent.c
	pthread_mutexattr_setkind_np.c
	pthread_mutexattr_getkind_np.c
	pthread_getw32threadhandle_np.c
	pthread_getunique_np.c
	pthread_delay_np.c
	pthread_num_processors_np.c
	pthread_win32_attach_detach_np.c
	pthread_equal.c
	pthread_getconcurrency.c
	pthread_once.c
	pthread_self.c
	pthread_setconcurrency.c
	pthread_rwlock_init.c
	pthread_rwlock_destroy.c
	pthread_rwlockattr_init.c
	pthread_rwlockattr_destroy.c
	pthread_rwlockattr_getpshared.c
	pthread_rwlockattr_setpshared.c
	pthread_rwlock_rdlock.c
	pthread_rwlock_wrlock.c
	pthread_rwlock_unlock.c
	pthread_rwlock_tryrdlock.c
	pthread_rwlock_trywrlock.c
	pthread_setschedparam.c
	pthread_getschedparam.c
	pthread_timechange_handler_np.c
	ptw32_is_attr.c
	ptw32_processInitialize.c
	ptw32_processTerminate.c
	ptw32_threadStart.c
	ptw32_threadDestroy.c
	ptw32_tkAssocCreate.c
	ptw32_tkAssocDestroy.c
	ptw32_callUserDestroyRoutines.c
	ptw32_timespec.c
	ptw32_throw.c
	ptw32_getprocessors.c
	ptw32_calloc.c
	ptw32_new.c
	ptw32_reuse.c
	ptw32_rwlock_check_need_init.c
	ptw32_cond_check_need_init.c
	ptw32_mutex_check_need_init.c
	ptw32_semwait.c
	ptw32_relmillisecs.c
	ptw32_MCS_lock.c
	sched_get_priority_max.c
	sched_get_priority_min.c
	sched_setscheduler.c
	sched_getscheduler.c
	sched_yield.c
	sem_init.c
	sem_destroy.c
	sem_trywait.c
	sem_timedwait.c
	sem_wait.c
	sem_post.c
	sem_post_multiple.c
	sem_getvalue.c
	sem_open.c
	sem_close.c
	sem_unlink.c
	signal.c
	pthread_kill.c
	ptw32_spinlock_check_need_init.c
	pthread_spin_init.c
	pthread_spin_destroy.c
	pthread_spin_lock.c
	pthread_spin_unlock.c
	pthread_spin_trylock.c
	pthread_detach.c
	pthread_join.c
	w32_CancelableWait.c
)

ADD_LIBRARY (pthreads ${SOURCE_FILES})
target_compile_definitions(pthreads PUBLIC ${PTHREAD_DEFINITIONS})

# Workaround for issue 0011240: VS 2010 Win64 does not correctly set /MACHINE:x64
# in the solution project files.
IF (CMAKE_CL_64)
  SET_TARGET_PROPERTIES (pthreads PROPERTIES STATIC_LIBRARY_FLAGS "/machine:x64")
ENDIF (CMAKE_CL_64)

set(CONF_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}")
set(CONF_DEFINITIONS "${PTHREAD_DEFINITIONS}")

configure_file(pthreads-config.cmake.in
  "${PROJECT_BINARY_DIR}/pthreads-config.cmake" @ONLY)

# Installation
INSTALL (TARGETS pthreads
  RUNTIME DESTINATION bin COMPONENT Runtime
  LIBRARY DESTINATION lib COMPONENT Runtime
  ARCHIVE DESTINATION lib COMPONENT Development
)
