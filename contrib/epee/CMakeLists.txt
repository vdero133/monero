# Copyright (c) 2014-2020, The Monero Project
# 
# All rights reserved.
# 
# Redistribution and use in source and binary forms, with or without modification, are
# permitted provided that the following conditions are met:
# 
# 1. Redistributions of source code must retain the above copyright notice, this list of
#    conditions and the following disclaimer.
# 
# 2. Redistributions in binary form must reproduce the above copyright notice, this list
#    of conditions and the following disclaimer in the documentation and/or other
#    materials provided with the distribution.
# 
# 3. Neither the name of the copyright holder nor the names of its contributors may be
#    used to endorse or promote products derived from this software without specific
#    prior written permission.
# 
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL
# THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,
# STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF
# THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

project(epee)
add_library(
        epee
        SHARED
        src/abstract_http_client.cpp
        src/buffer.cpp
        src/byte_slice.cpp
        src/byte_stream.cpp
        src/connection_basic.cpp
        src/file_io_utils.cpp
        src/hex.cpp
        src/http_auth.cpp
        src/http_base.cpp
        src/int-util.cpp
        src/levin_base.cpp
        src/memwipe.c
        src/misc_language.cpp
        src/misc_os_dependent.cpp
        src/mlocker.cpp
        src/mlog.cpp
        src/net_helper.cpp
        src/net_parse_helpers.cpp
        src/net_ssl.cpp
        src/net_utils_base.cpp
        src/network_throttle.cpp
        src/network_throttle-detail.cpp
        src/portable_storage.cpp
        src/readline_buffer.cpp
        src/string_tools.cpp
        src/tiny_ini.cpp
        src/wipeable_string.cpp

)

target_include_directories(
        epee
        PUBLIC
        include
)

target_link_libraries(epee
        PUBLIC
        easylogging
        Boost::chrono
        Boost::filesystem
        Boost::thread
        OpenSSL::SSL
        OpenSSL::Crypto
        )