project(delly)

get_filename_component(BIN "bin" ABSOLUTE)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${BIN}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${BIN}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${BIN}")


set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS_DEBUG "-g -O0")

# Get all the htslib c files
file(GLOB htslib_src "src/htslib/*.c")
file(GLOB htslib_h "src/htslib/*.h")

# Delly sources
file(GLOB delly_h "src/*.h")

find_package(ZLIB)
find_package(LibLZMA)
find_package(BZip2)
find_package(Boost 1.45.0 COMPONENTS filesystem iostreams system program_options date_time)
find_package(CURL)
find_library(htslib NAMES libhts.a)


add_executable(delly src/delly.cpp ${delly_h})
target_link_libraries(delly
        ${htslib}
        ZLIB::ZLIB
        ${LIBLZMA_LIBRARIES}
        ${BZIP2_LIBRARIES}
        Boost::iostreams
        Boost::filesystem
        Boost::system
        Boost::program_options
        Boost::date_time
        CURL::libcurl
        )
target_include_directories(delly
        PUBLIC
        "src/htslib"
        ${Boost_INCLUDE_DIRS}
        ${LIBLZMA_INCLUDE_DIRS}
        )

add_executable(dpe src/dpe.cpp ${delly_h})
target_link_libraries(dpe
        ${htslib}
        ZLIB::ZLIB
        ${LIBLZMA_LIBRARIES}
        ${BZIP2_LIBRARIES}
        Boost::iostreams
        Boost::filesystem
        Boost::system
        Boost::program_options
        Boost::date_time
        CURL::libcurl
        )
target_include_directories(dpe
        PUBLIC
        "src/htslib"
        ${Boost_INCLUDE_DIRS}
        ${LIBLZMA_INCLUDE_DIRS}
        )
