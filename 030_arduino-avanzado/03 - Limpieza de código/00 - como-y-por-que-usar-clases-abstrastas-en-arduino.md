> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-y-por-que-usar-clases-abstrastas-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplo 0
```cpp
#include "libs/AudioLibraries.hpp"
#include "libs/TFTLibraries.hpp"
#include "libs/SDLibraries.hpp"

Audio_Hard audio;
SD_Hard sd;
TFT_Hard tft;

std::string CurrentSong;
std::vector<std::string> PlayList;
bool IsPlaying;

// Funciones de dibujar en la TFT
void DrawBackground()
{
  tft.DoSomething();
}

void DrawButtons() {}
void DrawList() {}

void Draw() {
  DrawBackground();
  DrawButtons();
  DrawList();
}

// Funciones de audio
void Init() {}

void Play()
{
  audio.DoSomething();
}

void Stop() {}
void NextSong() {}

void PrevSong() {}

// Funciones de SD
void LoadPlaylistFromSD()
{
  sd.DoSomething();
}

// Setup
void setup()
{
  // Iniciar el hardware
  Init();

  // Cargas playlist desde la SD  
  LoadPlaylistFromSD();
}

// Loop, reducida para el ejemplo
void loop()
{
  // Aquí haríamos las acciones a realizar
  // en el MP3 al pulsar botones, etc...
  Play();
  Stop();

  // FInal
  Draw();
}
```

```cpp
#pragma once

class Audio_Hard
{
public: 
  void DoSomething()
  {
  }
};
```


## Clase MP3Player
```cpp
#pragma once

#include "libs/AudioLibraries.hpp"
#include "libs/TFTLibraries.hpp"
#include "libs/SDLibraries.hpp"

class MP3Player
{
public:
  Audio_Hard audio;
  SD_Hard sd;
  TFT_Hard tft;

  std::string CurrentSong;
  std::vector<std::string> PlayList;
  bool IsPlaying;

  void DrawBackground()
  {
    tft.DoSomething();
  }

  void DrawButtons()
  {
  }

  void DrawList()
  {
  }

  void Draw()
  {
    DrawBackground();
    DrawButtons();
    DrawList();
  }

  void Init()
  {
  }

  void Play()
  {
    audio.DoSomething();
  }

  void Stop()
  {
  }

  void NextSong()
  {
  }

  void PrevSong()
  {
  }

  void LoadPlaylistFromSD()
  {
    sd.DoSomething();
  }
};
```

```cpp
#include "MP3Player.hpp"

MP3Player mp3Player;

void setup()
{
  mp3Player.Init();
  mp3Player.LoadPlaylistFromSD();
}

void loop()
{
  mp3Player.Play();
  mp3Player.Stop();

  mp3Player.Draw();
}
```


## Clase MP3PlayerUI
```cpp
#pragma once

class Mp3Player
{
public:
  Audio_Hard audio;

  std::string CurrentSong;
  std::vector<std::string> PlayList;
  bool IsPlaying;

  void Init()
  {
  }

  void Play()
  {
    audio.DoSomething();
  }

  void Stop()
  {
  }

  void NextSong()
  {
  }

  void PrevSong()
  {
  }
};
```

```cpp
#pragma once

#include "Mp3Player.hpp"

class Mp3PlayerUI
{
public:
  Mp3Player& _mp3Player;
  TFT_Hard& _tft;

  Mp3PlayerUI(Mp3Player& mp3Player, TFT_Hard& tft) : _mp3Player(mp3Player), _tft(tft)
  {
  }

  void DrawBackground()
  {
    tft.DoSomething();
  }

  void DrawList()
  {
    for (auto song : _mp3Player.PlayList)
    {
    }
  }

  void Draw()
  {
    DrawBackground();
    DrawList();
  }
};
```

```cpp
#include "libs/AudioLibraries.hpp"
#include "libs/TFTLibraries.hpp"
#include "libs/SDLibraries.hpp"

#include "Mp3Player.hpp"
#include "Mp3PlayerUI.hpp"

TFT_Hard tft;
SD_Hard sd;

Mp3Player mp3Player;
Mp3PlayerUI mp3PlayerUI(mp3Player, tft);

void setup()
{
  mp3Player.Init();
}

void loop()
{
  mp3Player.Play();
  mp3Player.Stop();
  
  mp3PlayerUI.Draw();
}
```


## Clase abstracta IMP3Player
```cpp
#pragma once

class IMp3Player
{
public:
  std::string CurrentSong;
  std::vector<std::string> PlayList;
  bool IsPlaying;

  virtual void Init() = 0;
  virtual void Play() = 0;
  virtual void Stop() = 0;
  virtual void NextSong() = 0;
  virtual void PrevSong() = 0;
};
```

```cpp
#pragma once

#include "IMp3Player.hpp"

class Mp3Player : public IMp3Player
{
public:
  Audio_Hard audio;

  std::string CurrentSong;
  std::vector<std::string> PlayList;
  bool IsPlaying;

  void Init()
  {
  }

  void Play()
  {
    audio.DoSomething();
  }

  void Stop()
  {
  }

  void NextSong()
  {
  }

  void PrevSong()
  {
  }
};
```

```cpp
#pragma once

#include "IMp3Player.hpp"

class Mp3PlayerUI
{
public:
  IMp3Player& _mp3Player;
  TFT_Hard& _tft;

  Mp3PlayerUI(IMp3Player& mp3Player, TFT_Hard& tft) : _mp3Player(mp3Player), _tft(tft)
  {
  }

  void DrawBackground()
  {
    tft.DoSomething();
  }

  void DrawList()
  {
    for (auto song : _mp3Player.PlayList)
    {
    }
  }

  void Draw()
  {
    DrawBackground();
    DrawList();
  }
};
```

```cpp
#pragma once

#include "IMp3Player.hpp"

class Mp3Player_Mock : public IMp3Player
{

public:
  void Init()
  {
    Serial.println("Init");
  }

  void Play()
  {
    Serial.println("Play");
  }

  void Stop()
  {
    Serial.println("Stop");
  }

  void NextSong()
  {
    Serial.println("NextSong");
  }

  void PrevSong()
  {
    Serial.println("PrevSong");
  }

  void LoadPlaylistFromSD()
  {
    Serial.println("LoadPlaylistFromSD");
  }
};
```


## Servicio SDFiller
```cpp
#pragma once

class IPlaylistFillerService
{
public:
  virtual void FillPlaylist() = 0;
};

class ServiceMp3PlayerSdLoader : public IPlaylistFillerService
{
public:
  IMp3Player& _mp3;
  SD_Hard& _sd;

  ServiceMp3PlayerSdLoader(IMp3Player mp3, SD_Hard sd) : _mp3(mp3), _sd(sd)
  {
  }
  
  void FillPlaylist()
  {
    _mp3.PlayList;
    _sd.DoSomething();
  }
};
```

```cpp
#include "libs/AudioLibraries.hpp"
#include "libs/TFTLibraries.hpp"
#include "libs/SDLibraries.hpp"

#include "Mp3Player.hpp"
#include "Mp3PlayerUI.hpp"
#include "ServiceMp3PlayerSdLoader.hpp"

TFT_Hard tft;
SD_Hard sd;

Mp3Player mp3Player;
Mp3PlayerUI mp3PlayerUI(mp3Player, tft);
ServiceMp3PlayerSdLoader serviceMp3PlayerSdLoader(mp3Player, sd);

void setup()
{
  mp3Player.Init();
  serviceMp3PlayerSdLoader.FillPlaylist();
}

void loop()
{
  mp3Player.Play();
  mp3Player.Stop();
  
  mp3PlayerUI.Draw();
}
```


