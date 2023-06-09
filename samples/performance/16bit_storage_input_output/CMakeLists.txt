# Copyright (c) 2020-2021, Arm Limited and Contributors
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 the "License";
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
#

get_filename_component(FOLDER_NAME ${CMAKE_CURRENT_LIST_DIR} NAME)
get_filename_component(PARENT_DIR ${CMAKE_CURRENT_LIST_DIR} PATH)
get_filename_component(CATEGORY_NAME ${PARENT_DIR} NAME)

add_sample_with_tags(
        ID ${FOLDER_NAME}
        CATEGORY ${CATEGORY_NAME}
        AUTHOR "Hans-Kristian Arntzen"
        NAME "16-bit Storage InputOutput"
        DESCRIPTION "Using VK_KHR_16bit_storage InputOutput feature to reduce bandwidth for varying data."
        TAGS
        "arm"
        SHADER_FILES_GLSL
                "16bit_storage_input_output/16bit_storage_input_output_enabled.vert"
                "16bit_storage_input_output/16bit_storage_input_output_enabled.frag"
                "16bit_storage_input_output/16bit_storage_input_output_disabled.vert"
                "16bit_storage_input_output/16bit_storage_input_output_disabled.frag")
