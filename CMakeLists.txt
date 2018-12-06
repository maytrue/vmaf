cmake_minimum_required(VERSION 3.8)
project(vmaf)

include_directories(${CMAKE_SOURCE_DIR}/feature/src
        ${CMAKE_SOURCE_DIR}/feature/src/common
        ${CMAKE_SOURCE_DIR}/feature/src/iqa
        )

set(FEATURE_SRC_PATH ${CMAKE_SOURCE_DIR}/feature/src)
set(FEATURE_COMMON_PATH ${FEATURE_SRC_PATH}/common)
set(FEATURE_IQA_PATH ${FEATURE_SRC_PATH}/iqa)

set(FEATURE
        ${FEATURE_SRC_PATH}/adm.c
        ${FEATURE_SRC_PATH}/adm_tools.c
        ${FEATURE_SRC_PATH}/all.c
        ${FEATURE_SRC_PATH}/ansnr.c
        ${FEATURE_SRC_PATH}/ansnr_tools.c
        ${FEATURE_SRC_PATH}/moment.c
        ${FEATURE_SRC_PATH}/moment_main.c
        ${FEATURE_SRC_PATH}/motion.c
        ${FEATURE_SRC_PATH}/ms_ssim.c
        ${FEATURE_SRC_PATH}/ms_ssim_main.c
        ${FEATURE_SRC_PATH}/psnr.c
        ${FEATURE_SRC_PATH}/psnr_main.c
        ${FEATURE_SRC_PATH}/psnr_tools.c
        ${FEATURE_SRC_PATH}/ssim.c
        ${FEATURE_SRC_PATH}/ssim_main.c
        ${FEATURE_SRC_PATH}/vif.c
        ${FEATURE_SRC_PATH}/vif_tools.c
        ${FEATURE_SRC_PATH}/vmaf_main.c
        ${FEATURE_COMMON_PATH}/alignment.c
        ${FEATURE_COMMON_PATH}/alloc.c
        ${FEATURE_COMMON_PATH}/blur_array.c
        ${FEATURE_COMMON_PATH}/convolution_avx.c
        ${FEATURE_COMMON_PATH}/convolution.c
        ${FEATURE_COMMON_PATH}/cpu.c
        ${FEATURE_COMMON_PATH}/file_io.c
        ${FEATURE_COMMON_PATH}/frame.c
        ${FEATURE_IQA_PATH}/convolve.c
        ${FEATURE_IQA_PATH}/decimate.c
        ${FEATURE_IQA_PATH}/math_utils.c
        ${FEATURE_IQA_PATH}/ssim_tools.c
        )

add_library(feature ${FEATURE})

set(LIBSVM_SRC_PATH ${CMAKE_SOURCE_DIR}/libsvm)
add_library(libsvm ${LIBSVM_SRC_PATH}/svm.cpp)

include_directories(${LIBSVM_SRC_PATH})
add_executable(svm-train ${LIBSVM_SRC_PATH}/svm-train.c)
target_link_libraries(svm-train libsvm)

include_directories(${LIBSVM_SRC_PATH})
add_executable(svm-scale ${LIBSVM_SRC_PATH}/svm-scale.c)
target_link_libraries(svm-scale libsvm)

include_directories(${LIBSVM_SRC_PATH})
add_executable(svm-predict ${LIBSVM_SRC_PATH}/svm-predict.c)
target_link_libraries(svm-predict ${svm})
