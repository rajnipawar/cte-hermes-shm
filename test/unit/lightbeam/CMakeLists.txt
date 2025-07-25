find_package(PkgConfig REQUIRED)
pkg_check_modules(ZeroMQ REQUIRED libzmq)
pkg_check_modules(Thallium REQUIRED thallium)
pkg_check_modules(Libfabric REQUIRED libfabric)

if(HSHM_ENABLE_ZMQ)
    add_executable(lightbeam_transport_test lightbeam_transport_test.cc)
    add_dependencies(lightbeam_transport_test hermes_shm_host)
    target_include_directories(lightbeam_transport_test PRIVATE ${ZeroMQ_INCLUDE_DIRS} ${Thallium_INCLUDE_DIRS} ${Libfabric_INCLUDE_DIRS})
    target_link_directories(lightbeam_transport_test PRIVATE ${ZeroMQ_LIBRARY_DIRS} ${Thallium_LIBRARY_DIRS} ${Libfabric_LIBRARY_DIRS})
    target_link_libraries(lightbeam_transport_test hermes_shm_host ${ZeroMQ_LIBRARIES} ${Thallium_LIBRARIES} ${Libfabric_LIBRARIES})
    target_compile_definitions(lightbeam_transport_test PRIVATE HSHM_ENABLE_ZMQ)
    add_test(NAME lightbeam_transport_test COMMAND lightbeam_transport_test)
    install(TARGETS lightbeam_transport_test
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
endif()

add_executable(distributed_lightbeam_test distributed_lightbeam_test.cc)
add_dependencies(distributed_lightbeam_test hermes_shm_host)
target_include_directories(distributed_lightbeam_test PRIVATE ${ZeroMQ_INCLUDE_DIRS} ${Thallium_INCLUDE_DIRS} ${Libfabric_INCLUDE_DIRS})
target_link_directories(distributed_lightbeam_test PRIVATE ${ZeroMQ_LIBRARY_DIRS} ${Thallium_LIBRARY_DIRS} ${Libfabric_LIBRARY_DIRS})
target_link_libraries(distributed_lightbeam_test hermes_shm_host ${ZeroMQ_LIBRARIES} ${Thallium_LIBRARIES} ${Libfabric_LIBRARIES} ${MPI_LIBS} $<$<BOOL:${HSHM_RPC_THALLIUM}>:thallium>)
install(TARGETS distributed_lightbeam_test
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

