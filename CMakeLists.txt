#
# Copyright (C) 2014 Mathias Panzenböck
# Copyright Andrew Betson.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Lesser General Public License for more details.
#
# You should have received a copy of the Lesser GNU General Public License
# along with this program. If not, see <https://www.gnu.org/licenses/>.
#

CMake_Minimum_Required( VERSION 3.16.0 )

Project( vtfthumbcreator
	LANGUAGES CXX
	VERSION 1.0.0
)

Set( QT_MIN_VERSION 5.2.0 )

Find_Package( ECM 1.0.0 REQUIRED NO_MODULE )
Set( CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/cmake )

Include( FeatureSummary )
Include( WriteBasicConfigVersionFile )
Include( KDEInstallDirs )
Include( KDECMakeSettings )
Include( KDECompilerSettings NO_POLICY_SCOPE )

Find_Package( KF5 REQUIRED COMPONENTS KIO )
Find_Package( SharedMimeInfo REQUIRED )
FIND_PACKAGE(PkgConfig REQUIRED)
Pkg_Check_Modules( VTFLIB REQUIRED VTFLib )

File( GLOB_RECURSE VTFTHUMBCREATOR_SRC "${CMAKE_CURRENT_SOURCE_DIR}/code/**" )
Add_Library( vtfthumbcreator MODULE "src/VTFThumbCreator.cpp" )

Target_Include_Directories( vtfthumbcreator PRIVATE ${VTFLIB_INCLUDE_DIRS} )

Target_Link_Libraries( vtfthumbcreator
	KF5::KIOCore
	KF5::KIOWidgets
	"${VTFLIB_LIBRARIES}"
)

Install(
	TARGETS vtfthumbcreator
	DESTINATION "${PLUGIN_INSTALL_DIR}"
)

Install(
	FILES install/vtfthumbcreator.desktop
	DESTINATION "${SERVICES_INSTALL_DIR}"
)

Install(
	FILES install/x-vtf.xml
	DESTINATION "${KDE_INSTALL_MIMEDIR}"
)
Update_XDG_MimeTypes( "${KDE_INSTALL_MIMEDIR}" )
