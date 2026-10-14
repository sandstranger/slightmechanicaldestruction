cmake_minimum_required(VERSION 3.22.1)

project(SlightMechanicalDestruction LANGUAGES C)

set(CMAKE_C_STANDARD 23)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(SMD_SOURCES
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

add_library(slightmechanicaldestruction SHARED ${SMD_SOURCES})

set_target_properties(slightmechanicaldestruction PROPERTIES
    OUTPUT_NAME "game"
    PREFIX ""
    C_STANDARD 99
    C_STANDARD_REQUIRED YES
    C_EXTENSIONS YES
    C_VISIBILITY_PRESET default
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/release"
)

target_include_directories(slightmechanicaldestruction PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/src"
)

target_compile_options(slightmechanicaldestruction PRIVATE
    -Wall
    -fno-strict-aliasing
    -fomit-frame-pointer
    -fwrapv
)

target_link_libraries(slightmechanicaldestruction PRIVATE m)

foreach(source IN LISTS SMD_SOURCES)
    if(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/${source}")
        message(FATAL_ERROR "SMD source file does not exist: ${source}")
    endif()
endforeach()

message(STATUS "Building Slight Mechanical Destruction slightmechanicaldestruction module")
message(STATUS "Target ABI: ${ANDROID_ABI}")
message(STATUS "Output: ${CMAKE_BINARY_DIR}/release/slightmechanicaldestruction.so")
