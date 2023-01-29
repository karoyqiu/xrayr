#configure_file(resource.rc.in resource.rc @ONLY)

#set(TS_FILES xrayr_zh_CN.ts)

set(PROJECT_SOURCES
    main.cpp
    mainform.cpp mainform.h mainform.ui
    #resource.h "${CMAKE_CURRENT_BINARY_DIR}/resource.rc"
    #${TS_FILES}
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(xrayr WIN32
        ${PROJECT_SOURCES}
    )

    #qt_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})
else()
    if(ANDROID)
        add_library(xrayr SHARED
            ${PROJECT_SOURCES}
        )
    else()
        add_executable(xrayr WIN32
            ${PROJECT_SOURCES}
        )
    endif()

    #qt5_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})
endif()

set_directory_properties(PROPERTIES VS_STARTUP_PROJECT xrayr)
#target_sources(xrayr PRIVATE ${QM_FILES})
target_compile_definitions(xrayr
    PRIVATE	QT_DEPRECATED_WARNINGS QT_RESTRICTED_CAST_FROM_ASCII QT_NO_CAST_TO_ASCII
            QT_NO_CAST_FROM_BYTEARRAY QT_USE_QSTRINGBUILDER QT_NO_PROCESS_COMBINED_ARGUMENT_START
)

foreach(L ${QT_LIBS})
    target_precompile_headers(xrayr PRIVATE <Qt${L}>)
    target_link_libraries(xrayr PRIVATE Qt${QT_VERSION_MAJOR}::${L})
endforeach()
