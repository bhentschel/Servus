# Copyright (c) 2015, Human Brain Project
#                     Juan Hernando <jhernando@fi.upm.es>
#
# This file is part of Servus <https://github.com/HBPVIS/Servus>
#
# This library is free software; you can redistribute it and/or modify it under
# the terms of the GNU Lesser General Public License version 3.0 as published
# by the Free Software Foundation.
#
# This library is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
# FOR A PARTICULAR PURPOSE.  See the GNU Lesser General Public License for more
# details.
#
# You should have received a copy of the GNU Lesser General Public License
# along with this library; if not, write to the Free Software Foundation, Inc.,
# 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(Servus)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/CMake
                              ${CMAKE_SOURCE_DIR}/CMake/common)
include(GitExternal)

set(VERSION_MAJOR "1")
set(VERSION_MINOR "1")
set(VERSION_PATCH "0")
set(VERSION_ABI 2)

set(SERVUS_DESCRIPTION "A small set of network oriented utilities in C++ including a zeroconf implementation.")
set(COMMON_PROJECT_DOMAIN eu.humanbrainproject)

include(Common)

if(LINUX)
  common_package(avahi-client)
else()
  common_package(DNSSD)
endif()
common_package(Boost 1.51 COMPONENTS unit_test_framework)
common_package(Threads REQUIRED)
common_package_post()

add_subdirectory(servus)
add_subdirectory(tests)

include(CPackConfig)
set(DOXYGEN_EXTRA_INPUT ${PROJECT_SOURCE_DIR}/README.md)
set(DOXYGEN_MAINPAGE_MD README.md)
include(DoxygenRule) # must be after all targets
