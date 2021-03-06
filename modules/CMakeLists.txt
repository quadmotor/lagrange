#
# Copyright 2020 Adobe. All rights reserved.
# This file is licensed to you under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License. You may obtain a copy
# of the License at http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software distributed under
# the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR REPRESENTATIONS
# OF ANY KIND, either express or implied. See the License for the specific language
# governing permissions and limitations under the License.
#

# Collect subfolder names
file(GLOB module_paths "./*")
unset(module_names)
foreach(path IN ITEMS ${module_paths})
    if(IS_DIRECTORY "${path}")
        get_filename_component(name ${path} NAME)
        if(NOT (${name} STREQUAL "core" OR ${name} STREQUAL "ui"))
            list(APPEND module_names ${name})
        endif()
    endif()
endforeach()
# Include UI module first
list(PREPEND module_names "ui")

################################################################################

# Always build core library
add_subdirectory(core)
set_target_properties(lagrange_core PROPERTIES FOLDER "Lagrange")

# Iterate over each module folder and include it
foreach(name IN ITEMS ${module_names})
    lagrange_add_module(${name} "Build module lagrange::${name}")
endforeach()
