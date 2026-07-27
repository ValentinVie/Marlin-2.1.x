# Liens utiles : 
* https://3dprintscape.com/bltouch-on-skr-mini-install-guide/
* Télécharge Marlin & les fichiers de config associés: https://marlinfw.org/meta/download/

# Git & Astuces
* Source de la fork : https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.1.x
* Soource des fichiers de config défaut : https://github.com/MarlinFirmware/Configurations/tree/bugfix-2.1.x/config/examples/Creality/Ender-3%20Pro/BigTreeTech%20SKR%20Mini%20E3%202.0
* Installer l'extention platform IO code
* Pour lancer le build appuyer sur le petit logo tick en bas à droite (dans la bare du bas).
* Regarde le terminal à la fin du build, il y a le chemin du firmware `.pio/build/STM32F103RC_btt/firmware.bin`

# Champs à modifier : 
## Configuration.h
* `#define CUSTOM_MACHINE_NAME "Ender-3 Pro de Val"`
* E-steps `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 95.87 }`
* `#define BLTOUCH`
* Z-tuning`#define NOZZLE_TO_PROBE_OFFSET { -43, -8.4, -2.505 }`
* `#define MIN_SOFTWARE_ENDSTOP_Z`
* `#define AUTO_BED_LEVELING_BILINEAR`
* Commenter la ligne `#define MESH_BED_LEVELING`
* `#define GRID_MAX_POINTS_X 4`
* `#define Z_SAFE_HOMING`
* `#define INDIVIDUAL_AXIS_HOMING_MENU`

## Configuration_adv.h

* Block à éditer :
```
#if HAS_BED_PROBE && EITHER(HAS_MARLINUI_MENU, HAS_TFT_LVGL_UI)
  #define PROBE_OFFSET_WIZARD       //<----------// Add a Probe Z Offset calibration option to the LCD menu 
  #if ENABLED(PROBE_OFFSET_WIZARD)
    /**
     * Enable to init the Probe Z-Offset when starting the Wizard.
     * Use a height slightly above the estimated nozzle-to-probe Z offset.
     * For example, with an offset of -5, consider a starting height of -4.
     */
    #define PROBE_OFFSET_WIZARD_START_Z -3.0 //<----------
```
* Block à éditer :
 ```
  #define POWER_LOSS_RECOVERY //<----------
  #if ENABLED(POWER_LOSS_RECOVERY)
    #define PLR_ENABLED_DEFAULT   true //<---------- // Power Loss Recovery enabled by default. (Set with 'M413 Sn' & M500)
 ``` 
* Commenter `#define SDCARD_READONLY` (incompatible avec `POWER_LOSS_RECOVERY`)
* Block à éditer :
```
  #define BABYSTEP_DISPLAY_TOTAL          //<---------- // Display total babysteps since last G28,

  #define BABYSTEP_ZPROBE_OFFSET          //<---------- // Combine M851 Z and Babystepping,
  #if ENABLED(BABYSTEP_ZPROBE_OFFSET) 
    //#define BABYSTEP_HOTEND_Z_OFFSET      // For multiple hotends, babystep relative Z offsets
    //#define BABYSTEP_ZPROBE_GFX_OVERLAY   // Enable graphical overlay on Z-offset editor
  #endif
```

# platformio.ini
* `default_envs = STM32F103RC_btt`