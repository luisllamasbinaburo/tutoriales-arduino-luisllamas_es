> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/convertir-imagenes-para-arduino-con-lcd-image-conveter/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
    
    static const uint16_t image_data_myimage[76800] = {
        //... data ....
    };
    
    const tImage myImage = { image_data_myimage, 320, 240, 16};
```