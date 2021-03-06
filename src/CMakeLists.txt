#
# Copyright (c), University of Bologna and ETH Zurich
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
#			* Redistributions of source code must retain the above copyright notice, this
#				list of conditions and the following disclaimer.
#
#			* Redistributions in binary form must reproduce the above copyright notice,
#				this list of conditions and the following disclaimer in the documentation
#				and/or other materials provided with the distribution.
#
#			* Neither the name of the copyright holder nor the names of its
#				contributors may be used to endorse or promote products derived from
#				this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
# FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
# SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
# OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#
# Author: Daniele Cesarini, University of Bologna

# Source files
set(SOURCES
	init.c
	memory.c
	monitor.c
	log.c
	pm.c
	eam.c
	eam_slack.c
	wrapper_pmpi_c_cpp.c
	wrapper_pmpi_fortran.c
	tool.c
	calc.c
	share.c
	arch.c
	batch.c
	barrier.c
	andante.c
	fermata.c
	stacktrace/md5.c
	stacktrace/stacktrace.c
)

# Add dynamic library
add_library(cntd SHARED ${SOURCES})
target_link_libraries(cntd 
	PRIVATE
		cntd_mpi_c
		cntd_libmsr
		cntd_libunwind)

# Add static library
add_library(cntd-static STATIC ${SOURCES})
set_target_properties(cntd-static PROPERTIES OUTPUT_NAME "cntd")
target_link_libraries(cntd-static
	PRIVATE
		cntd_mpi_c
		cntd_libmsr
		cntd_libunwind)

# Add rpath for libmsr
set_target_properties(cntd 
    PROPERTIES
        INSTALL_RPATH "$ORIGIN/../external/libmsr/lib")

# Install cntd
install(
	TARGETS cntd cntd-static
	RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)