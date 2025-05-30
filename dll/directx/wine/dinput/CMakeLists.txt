
add_definitions(-DDIRECTINPUT_VERSION=0x0700)
spec2def(dinput.dll dinput.spec ADD_IMPORTLIB)

list(APPEND SOURCE
    config.c
    device.c
    dinput_main.c
    effect_linuxinput.c
    joystick.c
    joystick_linux.c
    joystick_linuxinput.c
    joystick_osx.c
    keyboard.c
    mouse.c
    precomp.h)

add_library(dinput MODULE
    ${SOURCE}
    data_formats.c
    dinput.rc
    version.rc
    ${CMAKE_CURRENT_BINARY_DIR}/dinput.def)
set_wine_module_FIXME(dinput) # CORE-5743: No ARRAY_SIZE macro

add_library(dinput_data_formats data_formats.c)
add_dependencies(dinput_data_formats psdk)
set_wine_module_FIXME(dinput_data_formats) # CORE-5743: No ARRAY_SIZE macro

set_module_type(dinput win32dll)
target_link_libraries(dinput dxguid uuid wine)
add_importlibs(dinput comctl32 ole32 user32 advapi32 msvcrt kernel32 ntdll)
add_pch(dinput precomp.h SOURCE)
add_cd_file(TARGET dinput DESTINATION reactos/system32 FOR all)
