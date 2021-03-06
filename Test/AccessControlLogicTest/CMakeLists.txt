# Copyright 2017 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#------------------------------------------------------------------------------
PROJECT(TEST_ACL VERSION 1)

SET(files
    test_acl_internal.cpp
    ${acl_dir}/access_control.cpp  
    ${acl_dir}/acl_internal.cpp
    ${acl_dir}/acl_read_write.cpp
    ${acl_dir}/acl_serialization.cpp
    ${acl_dir}/data_records_crypto.cpp
    ${common_dir}/PrivateLedger.cpp
    ${common_dir}/config.cpp
    ${common_dir}/safe_copy.cpp
)

# set properties for sgx generated file
set_source_files_properties( ${main_enclave_dir}/Enclave_t.h PROPERTIES GENERATED TRUE )

#declare executable 
add_executable(${PROJECT_NAME} ${files})

# add dependency on sgx generated files
add_dependencies(${PROJECT_NAME} generate-enclave-t)

#target_include_directories(${PROJECT_NAME} PUBLIC ${business_logic_dir})
target_include_directories(${PROJECT_NAME} PUBLIC ${acl_dir})
target_include_directories(${PROJECT_NAME} PUBLIC ${crypto_dir})
target_include_directories(${PROJECT_NAME} PUBLIC ${listener_enclave_dir})
target_include_directories(${PROJECT_NAME} PUBLIC $ENV{SGX_SDK}/include/)
target_include_directories(${PROJECT_NAME} PUBLIC ${common_dir})
target_include_directories(${PROJECT_NAME} PUBLIC ${main_enclave_dir})

# Link runTests with what we want to test and the GTest library
target_link_libraries(${PROJECT_NAME} GTest::Main)


set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)


