
include_directories(${REACTOS_SOURCE_DIR}/boot/freeldr)

if(ARCH STREQUAL "i386" OR ARCH STREQUAL "amd64")

    CreateBootSectorTarget(dosmbr ${CMAKE_CURRENT_SOURCE_DIR}/dosmbr.S ${CMAKE_CURRENT_BINARY_DIR}/dosmbr.bin 7c00)
    CreateBootSectorTarget(ext ${CMAKE_CURRENT_SOURCE_DIR}/ext.S ${CMAKE_CURRENT_BINARY_DIR}/ext.bin 7c00)
    CreateBootSectorTarget(extldr ${CMAKE_CURRENT_SOURCE_DIR}/extldr.S ${CMAKE_CURRENT_BINARY_DIR}/extldr.sys 8000)

    CreateBootSectorTarget(fat ${CMAKE_CURRENT_SOURCE_DIR}/fat.S ${CMAKE_CURRENT_BINARY_DIR}/fat.bin 7c00)
    CreateBootSectorTarget(fat32 ${CMAKE_CURRENT_SOURCE_DIR}/fat32.S ${CMAKE_CURRENT_BINARY_DIR}/fat32.bin 7c00)
    CreateBootSectorTarget(ntfsvbr ${CMAKE_CURRENT_SOURCE_DIR}/ntfs.S ${CMAKE_CURRENT_BINARY_DIR}/ntfs.bin 7c00)
    CreateBootSectorTarget(btrfsvbr ${CMAKE_CURRENT_SOURCE_DIR}/btrfs.S ${CMAKE_CURRENT_BINARY_DIR}/btrfs.bin 7c00)

    if(SARCH STREQUAL "pc98")
        CreateBootSectorTarget(fat12pc98 ${CMAKE_CURRENT_SOURCE_DIR}/pc98/fat12fdd.S ${CMAKE_CURRENT_BINARY_DIR}/pc98/fat12fdd.bin 7c00)
    endif()

    ## New versions using FATY.S (experimental)
    # add_definitions(-DFAT12)
    # CreateBootSectorTarget(fat_new ${CMAKE_CURRENT_SOURCE_DIR}/faty.S ${CMAKE_CURRENT_BINARY_DIR}/fat_new.bin 7c00)
    # remove_definitions(-DFAT12)
    # add_definitions(-DFAT16)
    # CreateBootSectorTarget(fat16_new ${CMAKE_CURRENT_SOURCE_DIR}/faty.S ${CMAKE_CURRENT_BINARY_DIR}/fat16_new.bin 7c00)
    # remove_definitions(-DFAT16)
    # add_definitions(-DFAT32)
    # CreateBootSectorTarget(fat32_new ${CMAKE_CURRENT_SOURCE_DIR}/faty.S ${CMAKE_CURRENT_BINARY_DIR}/fat32_new.bin 7c00)
    # remove_definitions(-DFAT32)

    CreateBootSectorTarget(isoboot ${CMAKE_CURRENT_SOURCE_DIR}/isoboot.S ${CMAKE_CURRENT_BINARY_DIR}/isoboot.bin 7000)
    CreateBootSectorTarget(isobtrt ${CMAKE_CURRENT_SOURCE_DIR}/isobtrt.S ${CMAKE_CURRENT_BINARY_DIR}/isobtrt.bin 7000)
    CreateBootSectorTarget(isombr ${CMAKE_CURRENT_SOURCE_DIR}/isombr.S ${CMAKE_CURRENT_BINARY_DIR}/isombr.bin 7000)

    add_cd_file(TARGET dosmbr DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/dosmbr.bin FOR bootcd regtest)
    add_cd_file(TARGET ext DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/ext.bin FOR bootcd regtest)
    add_cd_file(TARGET extldr DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/extldr.sys FOR bootcd regtest)
    add_cd_file(TARGET btrfsvbr DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/btrfs.bin FOR bootcd regtest)
    add_cd_file(TARGET fat DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/fat.bin FOR bootcd regtest)
    add_cd_file(TARGET fat32 DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/fat32.bin FOR bootcd regtest)
    add_cd_file(TARGET ntfsvbr DESTINATION loader NO_CAB FILE ${CMAKE_CURRENT_BINARY_DIR}/ntfs.bin FOR bootcd regtest)
    add_cd_file(TARGET isoboot DESTINATION loader NO_CAB NOT_IN_HYBRIDCD FILE ${CMAKE_CURRENT_BINARY_DIR}/isoboot.bin FOR all hybridcd)
    add_cd_file(TARGET isobtrt DESTINATION loader NO_CAB NOT_IN_HYBRIDCD FILE ${CMAKE_CURRENT_BINARY_DIR}/isobtrt.bin FOR bootcd regtest)
elseif(ARCH STREQUAL "arm")
    add_custom_target(fat
        COMMAND exit 1
        COMMENT "arm fat target not implemented yet")
    add_custom_target(fat32
        COMMAND exit 1
        COMMENT "arm fat32 target not implemented yet")
endif()
