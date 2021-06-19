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

include_directories(SYSTEM ${ZMQ_INCLUDE_PATH})

set(rpc_base_sources
        src/rpc_args.cpp
        src/rpc_payment_signature.cpp
        src/rpc_handler.cpp)

set(rpc_sources
        src/bootstrap_daemon.cpp
        src/bootstrap_node_selector.cpp
        src/core_rpc_server.cpp
        src/rpc_payment.cpp
        src/rpc_version_str.cpp
        src/instanciations.cpp)

set(daemon_messages_sources
        src/message.cpp
        src/daemon_messages.cpp)

set(rpc_pub_sources src/zmq_pub.cpp)

set(daemon_rpc_server_sources
        src/daemon_handler.cpp
        src/zmq_pub.cpp
        src/zmq_server.cpp)

add_library(rpc_base
        ${rpc_base_sources}
        )

add_library(rpc
        ${rpc_sources}
        )

add_library(rpc_pub
        ${rpc_pub_sources}
        )

add_library(daemon_messages
        ${daemon_messages_sources}
        )

add_library(daemon_rpc_server
        ${daemon_rpc_server_sources}
        )

target_include_directories(rpc_base
        PUBLIC
        include/)
target_link_libraries(rpc_base
        PUBLIC
        common
        epee
        Boost::regex
        Boost::thread
        Boost::program_options
        cryptonote_core
        PRIVATE
        ${EXTRA_LIBRARIES})
target_include_directories(rpc
        PUBLIC
        include/)
target_link_libraries(rpc
        PUBLIC
        rpc_base
        common
        cryptonote_core
        cryptonote_protocol
        net
        version
        Boost::regex
        Boost::thread
        PRIVATE
        ${EXTRA_LIBRARIES})
target_include_directories(rpc_pub
        PUBLIC
        include/)
target_link_libraries(rpc_pub
        PUBLIC
        epee
        net
        cryptonote_basic
        cryptonote_core
        serialization
        Boost::thread
)

target_link_libraries(daemon_messages
        LINK_PRIVATE
        cryptonote_core
        cryptonote_protocol
        version
        serialization
        ${EXTRA_LIBRARIES})

target_link_libraries(daemon_rpc_server
        LINK_PRIVATE
        rpc
        rpc_pub
        cryptonote_core
        cryptonote_protocol
        version
        daemon_messages
        serialization
        Boost::chrono
        Boost::regex
        Boost::system
        Boost::thread
        ZeroMQ::libzmq-static
        ${EXTRA_LIBRARIES})

target_include_directories(daemon_rpc_server PUBLIC ${ZMQ_INCLUDE_PATH})
