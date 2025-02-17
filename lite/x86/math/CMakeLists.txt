add_subdirectory(detail)

function(math_library TARGET)
    # math_library is a function to create math library.
    # The interface is the same as lite_cc_library.
    # But it handle split GPU/CPU code and link some common library.
    set(cc_srcs)
    set(hip_srcs)
    set(math_common_deps context framework_proto)
    set(multiValueArgs DEPS)
    cmake_parse_arguments(math_library "${options}" "${oneValueArgs}"
            "${multiValueArgs}" ${ARGN})

    if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/${TARGET}.cc)
        list(APPEND cc_srcs ${TARGET}.cc)
    endif()

    list(LENGTH cc_srcs cc_srcs_len)
    lite_cc_library(${TARGET} SRCS ${cc_srcs} DEPS ${math_library_DEPS} ${math_common_deps} eigen3)
endfunction()

# please add new math_library in alphabetical order
math_library(concat_and_split)
math_library(context_project DEPS im2col math_function)
math_library(cross_entropy)
math_library(cos_sim_functor)
## math_library(depthwise_conv DEPS cub)
math_library(im2col)
math_library(sample_prob)
math_library(sampler)

math_library(gru_compute DEPS activation_functions math_function)
## math_library(lstm_compute DEPS activation_functions)

lite_cc_library(blas SRCS blas.cc DEPS cblas framework_proto eigen3)
math_library(math_function DEPS blas)
math_library(maxouting)
math_library(pooling)
# math_library(selected_rows_functor DEPS selected_rows math_function blas)
math_library(sequence2batch)
math_library(sequence_padding)
math_library(sequence_pooling DEPS math_function jit_kernel_helper)
math_library(sequence_scale)
math_library(softmax DEPS math_function jit_kernel_helper)
math_library(beam_search DEPS math_function)
#
## math_library(matrix_bit_code)
#
math_library(unpooling)
math_library(vol2col)
## math_library(prelu)
math_library(tree2col DEPS math_function)

# cc_test(math_function_test SRCS math_function_test.cc DEPS math_function)
# cc_test(selected_rows_functor_test SRCS selected_rows_functor_test.cc DEPS selected_rows_functor)
# cc_test(im2col_test SRCS im2col_test.cc DEPS im2col)
# cc_test(vol2col_test SRCS vol2col_test.cc DEPS vol2col)
# cc_test(sequence_padding_test SRCS sequence_padding_test.cc DEPS sequence_padding)
# cc_test(sequence_pooling_test SRCS sequence_pooling_test.cc DEPS sequence_pooling)
# cc_test(beam_search_test SRCS beam_search_test.cc DEPS beam_search)
# cc_test(concat_test SRCS concat_test.cc DEPS concat_and_split)
# cc_test(cpu_vec_test SRCS cpu_vec_test.cc DEPS blas cpu_info)
