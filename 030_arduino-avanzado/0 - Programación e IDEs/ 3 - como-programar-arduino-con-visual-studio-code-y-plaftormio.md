> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-programar-arduino-con-visual-studio-code-y-plaftormio/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Crear un nuevo proyecto
```bash
project_dir
├── lib
│   └── README
├── platformio.ini
└── src
    └── main.cpp
```



## El fichero platform.ini
```bash
[env:m5stack-atom]
platform = espressif32
board = m5stack-atom
framework = arduino
```


