cmake_minimum_required(VERSION 3.5.1)

# Print a message that using the Makefiles is recommended.
message(NOTICE: " The CMakeLists.txt is unmaintained. Use the Makefile if possible.")

# Enforce "Debug" as standard build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose the type of build, options are: Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif(NOT CMAKE_BUILD_TYPE)

# CMake project configuration
project(yquake2-smd)

# Enforce compiler flags (GCC / Clang compatible, yquake2
# won't build with another compiler anyways)
#  -Wall                -> More warnings
#  -fno-strict-aliasing -> Quake 2 is far away from strict aliasing
#  -fwrapv              -> Make signed integer overflows defined
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -fno-strict-aliasing -fwrapv")

# Use -O2 as maximum optimization level. -O3 has it's problems with yquake2.
string(REPLACE "-O3" "-O2" CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE}")

# Operating system
add_definitions(-DYQ2OSTYPE="${CMAKE_SYSTEM_NAME}")

# Architecture string
string(REGEX REPLACE "amd64" "x86_64" YQ2_ARCH "${CMAKE_SYSTEM_PROCESSOR}")
string(REGEX REPLACE "i.86" "i386" YQ2_ARCH "${YQ2_ARCH}")
string(REGEX REPLACE "^arm.*" "arm" YQ2_ARCH "${YQ2_ARCH}")
add_definitions(-DYQ2ARCH="${YQ2_ARCH}")

# Linker Flags
if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    list(APPEND smdLinkerFlags "-lm")
else()
    list(APPEND smdLinkerFlags "-lm -rdynamic")
endif()

set(smd-Source
        src/g_ai.c
        src/g_camera.c
        src/g_chase.c
        src/g_cmds.c
        src/g_combat.c
        src/g_crane.c
        src/g_fog.c
        src/g_func.c
        src/g_items.c
        src/g_jetpack.c
        src/g_lights.c
        src/g_lock.c
        src/g_main.c
        src/g_misc.c
        src/g_model.c
        src/g_monster.c
        src/g_mtrain.c
        src/g_newai.c
        src/g_patchplayermodels.c
        src/g_pendulum.c
        src/g_phys.c
        src/g_reflect.c
        src/g_save.c
        src/g_sound.c
        src/g_spawn.c
        src/g_svcmds.c
        src/g_target.c
        src/g_thing.c
        src/g_tracktrain.c
        src/g_trigger.c
        src/g_turret.c
        src/g_utils.c
        src/g_vehicle.c
        src/g_weapon.c
        src/m_actor_weap.c
        src/m_actor.c
        src/m_berserk.c
        src/m_boss2.c
        src/m_boss3.c
        src/m_boss31.c
        src/m_boss32.c
        src/m_brain.c
        src/m_chick.c
        src/m_flash.c
        src/m_flipper.c
        src/m_float.c
        src/m_flyer.c
        src/m_gladiator.c
        src/m_gunner.c
        src/m_hover.c
        src/m_infantry.c
        src/m_insane.c
        src/m_medic.c
        src/m_move.c
        src/m_mutant.c
        src/m_parasite.c
        src/m_sentrybot.c
        src/m_soldier.c
        src/m_supertank.c
        src/m_tank.c
        src/p_client.c
        src/p_chase.c
        src/p_hud.c
        src/p_menu.c
        src/p_text.c
        src/p_trail.c
        src/p_view.c
        src/p_weapon.c
        src/q_shared.c
)

# Build the smd dynamic library
add_library(smd SHARED ${smd-Source})
target_link_libraries(smd ${smdLinkerFlags})
