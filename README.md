ESP_Panel_Board_Custom.h needs to be configured.

## LCD Panel's [Name,Width,Height] config
``` c
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//////////////////////////// Please update the following macros to configure the LCD panel /////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
/* Set to 1 when using an LCD panel */
#define ESP_PANEL_USE_LCD           (1)     // 0/1

#if ESP_PANEL_USE_LCD
/**
 * LCD Controller Name. Choose one of the following:
 *      - GC9A01, GC9B71, GC9503
 *      - ILI9341
 *      - NV3022B
 *      - SH8601
 *      - SPD2010
 *      - ST7262, ST7701, ST7789, ST7796, ST77916, ST77922
 */
#define ESP_PANEL_LCD_NAME          ST7796

/* LCD resolution in pixels */
#define ESP_PANEL_LCD_WIDTH         (320)
#define ESP_PANEL_LCD_HEIGHT        (480)
```

## LCD Panel's [Init Cmd, Color, Swap, Reset pin] config
```c
/**
 * LCD Vendor Initialization Commands.
 *
 * Vendor specific initialization can be different between manufacturers, should consult the LCD supplier for
 * initialization sequence code. Please uncomment and change the following macro definitions. Otherwise, the LCD driver
 * will use the default initialization sequence code.
 *
 * There are two formats for the sequence code:
 *   1. Raw data: {command, (uint8_t []){ data0, data1, ... }, data_size, delay_ms}
 *   2. Formater: ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(delay_ms, command, { data0, data1, ... }) and
 *                ESP_PANEL_LCD_CMD_WITH_NONE_PARAM(delay_ms, command)
 */
// #define ESP_PANEL_LCD_VENDOR_INIT_CMD()                                        \
//     {                                                                          \
//         {0xFF, (uint8_t []){0x77, 0x01, 0x00, 0x00, 0x10}, 5, 0},              \
//         {0xC0, (uint8_t []){0x3B, 0x00}, 2, 0},                                \
//         {0xC1, (uint8_t []){0x0D, 0x02}, 2, 0},                                \
//         {0x29, (uint8_t []){0x00}, 0, 120},                                    \
//         or                                                                     \
//         ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xFF, {0x77, 0x01, 0x00, 0x00, 0x10}), \
//         ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xC0, {0x3B, 0x00}),                   \
//         ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xC1, {0x0D, 0x02}),                   \
//         ESP_PANEL_LCD_CMD_WITH_NONE_PARAM(120, 0x29),                               \
//     }

#define ESP_PANEL_LCD_VENDOR_INIT_CMD() \
{ \
    ESP_PANEL_LCD_CMD_WITH_NONE_PARAM(120, 0x01), \
    ESP_PANEL_LCD_CMD_WITH_NONE_PARAM(120, 0x11), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(120, 0xF0, {0xC3}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xF0, {0x96}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0x36, {0x48}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0x3A, {0x55}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xB4, {0x01}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xB6, {0x80,0x02,0x3B}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xE8, {0x40,0x8A,0x00,0x00,0x29,0x19,0xA5,0x33}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xC1, {0x06}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xC2, {0xA7}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(0, 0xC5, {0x18}), \
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(120, 0xE0, {0xF0,0x09,0x0b,0x06,0x04,0x15,0x2F,0x54,0x42,0x3C,0x17,0x14,0x18,0x1B}), \            
    ESP_PANEL_LCD_CMD_WITH_8BIT_PARAM(120, 0xE1, {0xE0,0x09,0x0B,0x06,0x04,0x03,0x2B,0x43,0x42,0x3B,0x16,0x14,0x17,0x1B}), \                   
    ESP_PANEL_LCD_CMD_WITH_NONE_PARAM(120, 0x29), \
}

/* LCD Color Settings */
/* LCD color depth in bits */
#define ESP_PANEL_LCD_COLOR_BITS    (16)        // 8/16/18/24
/*
 * LCD RGB Element Order. Choose one of the following:
 *      - 0: RGB
 *      - 1: BGR
 */
#define ESP_PANEL_LCD_BGR_ORDER     (1)         // 0/1
#define ESP_PANEL_LCD_INEVRT_COLOR  (1)         // 0/1

/* LCD Transformation Flags */
#define ESP_PANEL_LCD_SWAP_XY       (0)         // 0/1
#define ESP_PANEL_LCD_MIRROR_X      (1)         // 0/1
#define ESP_PANEL_LCD_MIRROR_Y      (0)         // 0/1

/* LCD Other Settings */
/* IO num of RESET pin, set to -1 if not use */
#define ESP_PANEL_LCD_IO_RST        (22)
#define ESP_PANEL_LCD_RST_LEVEL     (0)         // 0: low level, 1: high level

#endif /* ESP_PANEL_USE_LCD */
```

## LCD Touch's [Name,SCL,SDA] config
```c
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//////////////////////////// Please update the following macros to configure the touch panel ///////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
/* Set to 1 when using an touch panel */
#define ESP_PANEL_USE_TOUCH         (1)         // 0/1
#if ESP_PANEL_USE_TOUCH
/**
 * Touch controller name. Choose one of the following:
 *      - CST816S
 *      - FT5x06
 *      - GT911, GT1151
 *      - ST1633, ST7123
 *      - TT21100
 *      - XPT2046
 */
#define ESP_PANEL_TOUCH_NAME        FT5x06

/* Touch resolution in pixels */
#define ESP_PANEL_TOUCH_H_RES       (ESP_PANEL_LCD_WIDTH)   // Typically set to the same value as the width of LCD
#define ESP_PANEL_TOUCH_V_RES       (ESP_PANEL_LCD_HEIGHT)  // Typically set to the same value as the height of LCD

/* Touch Panel Bus Settings */
/**
 * If set to 1, the bus will skip to initialize the corresponding host. Users need to initialize the host in advance.
 * It is useful if other devices use the same host. Please ensure that the host is initialized only once.
 */
#define ESP_PANEL_TOUCH_BUS_SKIP_INIT_HOST      (0)     // 0/1
/**
 * Touch panel bus type. Choose one of the following:
 *      - ESP_PANEL_BUS_TYPE_I2C
 *      - ESP_PANEL_BUS_TYPE_SPI
 */
#define ESP_PANEL_TOUCH_BUS_TYPE            (ESP_PANEL_BUS_TYPE_I2C)
/* Touch panel bus parameters */
#if ESP_PANEL_TOUCH_BUS_TYPE == ESP_PANEL_BUS_TYPE_I2C

    #define ESP_PANEL_TOUCH_BUS_HOST_ID     (0)     // Typically set to 0
    #define ESP_PANEL_TOUCH_I2C_ADDRESS     (0x38)     // Typically set to 0 to use default address
#if !ESP_PANEL_TOUCH_BUS_SKIP_INIT_HOST
    #define ESP_PANEL_TOUCH_I2C_CLK_HZ      (400 * 1000)
                                                    // Typically set to 400K
    #define ESP_PANEL_TOUCH_I2C_SCL_PULLUP  (1)     // 0/1
    #define ESP_PANEL_TOUCH_I2C_SDA_PULLUP  (1)     // 0/1
    #define ESP_PANEL_TOUCH_I2C_IO_SCL      (19)
    #define ESP_PANEL_TOUCH_I2C_IO_SDA      (18)
```

## LCD Backlight config
```c
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
///////////////////////////// Please update the following macros to configure the backlight ////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
#define ESP_PANEL_USE_BACKLIGHT         (1)         // 0/1
#if ESP_PANEL_USE_BACKLIGHT
/* IO num of backlight pin */
#define ESP_PANEL_BACKLIGHT_IO          (23)
#define ESP_PANEL_BACKLIGHT_ON_LEVEL    (1)         // 0: low level, 1: high level

/* Set to 1 if you want to turn off the backlight after initializing the panel; otherwise, set it to turn on */
#define ESP_PANEL_BACKLIGHT_IDLE_OFF    (0)         // 0: on, 1: off

/* Set to 1 if use PWM for brightness control */
#define ESP_PANEL_LCD_BL_USE_PWM        (1)         // 0/1
#endif /* ESP_PANEL_USE_BACKLIGHT */
```





