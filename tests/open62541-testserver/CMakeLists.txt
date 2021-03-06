# Generated from open62541-testserver.pro.

#####################################################################
## open62541-testserver Binary:
#####################################################################

qt_add_executable(open62541-testserver
    SOURCES
        ../../src/plugins/opcua/open62541/qopen62541utils.cpp
        ../../src/plugins/opcua/open62541/qopen62541valueconverter.cpp
        main.cpp
        testserver.cpp testserver.h
    INCLUDE_DIRECTORIES
        ../../src/plugins/opcua/open62541
    OUTPUT_DIRECTORY # special case
        "${CMAKE_CURRENT_BINARY_DIR}" # special case
    PUBLIC_LIBRARIES
        Qt::Gui
        Qt::OpcUaPrivate
)

# special case begin
set_target_properties(open62541-testserver PROPERTIES MACOSX_BUNDLE TRUE)
# special case end

# Resources:
set(certs_resource_files
    "pki/own/certs/open62541-testserver.der"
    "pki/own/private/open62541-testserver.der"
    "pki/trusted/certs/opcuaviewer.der"
    "pki/trusted/certs/tst_security.der"
)

qt_add_resource(open62541-testserver "certs"
    PREFIX
        "/"
    FILES
        ${certs_resource_files}
)


#### Keys ignored in scope 1:.:.:open62541-testserver.pro:<TRUE>:
# DEPENDPATH = "INCLUDEPATH"
# QMAKE_CFLAGS_RELEASE = "--O2"
# QMAKE_CFLAGS_RELEASE_WITH_DEBUGINFO = "--O2"
# TEMPLATE = "app"

# special case begin
# We need to disable optimizations on MSVC, to circumvent buggy code in the open62541.c file.
# Otherwise the test server fails to launch on Windows MSVC with UA_STATUSCODE_BADOUTOFMEMORY.
# We also disable them on Unixy platforms, to mimic the qmake fix done in QTBUG-75020.
# See QTBUG-85939 for details.
qt_opcua_disable_optimizations_in_current_dir()
# special case end

## Scopes:
#####################################################################

qt_extend_target(open62541-testserver CONDITION QT_FEATURE_open62541 AND NOT QT_FEATURE_system_open62541
    SOURCES
        ../../src/3rdparty/open62541/open62541.h
    ../../src/3rdparty/open62541/open62541.c # special case
    INCLUDE_DIRECTORIES
        ../../src/3rdparty/open62541
)

qt_extend_target(open62541-testserver CONDITION QT_FEATURE_mbedtls AND QT_FEATURE_open62541 AND NOT QT_FEATURE_system_open62541
    DEFINES
        UA_ENABLE_ENCRYPTION
    LIBRARIES
        mbedtls
)

qt_extend_target(open62541-testserver CONDITION QT_FEATURE_open62541 AND WIN32 AND MSVC AND NOT QT_FEATURE_system_open62541 # special case
    PUBLIC_LIBRARIES
        ws2_32.lib
)

qt_extend_target(open62541-testserver CONDITION (QT_FEATURE_open62541 AND NOT QT_FEATURE_system_open62541) AND (WIN32 AND GCC) # special case
    PUBLIC_LIBRARIES
        ws2_32
)

# special case begin
#qt_extend_target(open62541-testserver CONDITION QT_FEATURE_open62541 AND NOT QT_FEATURE_system_open62541 AND (WINRT OR WIN32 AND MSVC)
#    SOURCES
#        ../../src/3rdparty/open62541/open62541.c
#)
# special case end

# special case begin
if (QT_FEATURE_open62541 AND NOT QT_FEATURE_system_open62541 AND NOT (WINRT OR WIN32 AND MSVC)) # special case
    set_source_files_properties(../../src/3rdparty/open62541/open62541.c PROPERTIES
        COMPILE_OPTIONS
            "-Wno-unused-parameter;-Wno-unused-function;-Wno-format;-Wno-strict-aliasing;-Wno-unused-result;-std=c99")
endif()
# special case end

#### Keys ignored in scope 10:.:../../src/3rdparty:../../src/3rdparty/open62541.pri:else:
# OPEN62541_CFLAGS = "-Wno-unused-parameter" "-Wno-unused-function" "-Wno-format" "-Wno-strict-aliasing" "-Wno-unused-result" "-std=c99"
# QMAKE_EXTRA_COMPILERS = "open62541_compiler"
# open62541_compiler.commands = "$$QMAKE_CC" "-c" "$(CFLAGS)" "$$OPEN62541_CFLAGS" "$(INCPATH)" "${QMAKE_FILE_IN}" "-o" "${QMAKE_FILE_OUT}"
# open62541_compiler.dependency_type = "TYPE_C"
# open62541_compiler.input = "OPEN62541_SOURCES"
# open62541_compiler.name = "compiling" "${QMAKE_FILE_IN}"
# open62541_compiler.output = "${QMAKE_VAR_OBJECTS_DIR}${QMAKE_FILE_BASE}$${firstQMAKE_EXT_OBJ}"

#### Keys ignored in scope 11:.:../../src/3rdparty:../../src/3rdparty/open62541.pri:silent:
# open62541_compiler.commands = "@echo" "compiling" "${QMAKE_FILE_IN}" "&&"

qt_extend_target(open62541-testserver CONDITION QT_FEATURE_system_open62541 OR NOT QT_FEATURE_open62541
    LIBRARIES
        open62541
)

#### Keys ignored in scope 5:.:.:open62541-testserver.pro:WIN32:
# DESTDIR = "./"
