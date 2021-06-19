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

add_library(cncrypto
        STATIC
        src/aesb.c
        src/blake256.c
        src/chacha.c
        src/crypto.cpp
        src/CryptonightR_JIT.c
        src/crypto-ops.c
        src/crypto-ops-data.c
        src/groestl.c
        src/hash.c
        src/hash-extra-blake.c
        src/hash-extra-groestl.c
        src/hash-extra-jh.c
        src/hash-extra-skein.c
        src/hmac-keccak.c
        src/jh.c
        src/keccak.c
        src/oaes_lib.c
        src/random.c
        src/rx-slow-hash.c
        src/skein.c
        src/slow-hash.c
        src/tree-hash.c)
target_include_directories(cncrypto
        PUBLIC
        include/
        )
target_link_libraries(cncrypto
  PUBLIC
    epee
    randomx
    Boost::system
    ${SODIUM_LIBRARY}
  PRIVATE
        config
    ${EXTRA_LIBRARIES})

target_architecture(ARCH)
if(${ARCH} STREQUAL "armv8")
    target_compile_options(cncrypto PUBLIC -march=armv8-a+crypto)
elseif(${ARCH} STREQUAL "i386" OR ${ARCH} STREQUAL "x86_64" OR ${ARCH} STREQUAL "x86-64" OR ${ARCH} STREQUAL "amd64")
    target_compile_options(cncrypto PUBLIC -maes)
endif()


add_library(wallet-crypto ALIAS cncrypto)
