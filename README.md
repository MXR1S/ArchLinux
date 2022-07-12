# Вступление
Здравствуй уважаемый читатель данной статьи. В ней я хочу поделиться с тобой небольшой информацией, как я настраиваю Arch для повседневного использования и по большей части - игр. Прежде чем мы начнём хочу сказать тебе, что **ВСЕ ДЕЙСТИВЯ ВЫПОЛНЯЮТСЯ НА СВОЙ СТРАХ И РИСК**, но ты всегда можешь обраться ко мне в дискорд за помощью - __MXRIS#5353__.

Так же если тебе есть что добавить в статью ты можешь всегда связаться со мной в дискорде и мы это добавим или исправим!

## Базовые комманды
Эти комманды предназначены для работы с пакетным менеджером, рекомендую их запомнить, ими ты будешь пользоваться очень часто:
~~~c++
sudo pacman -S    /// Установка
sudo pacman -Syyu /// Полная синхронизация/поиск обновлений пакетов
sudo pacman -R    /// Удаление   
sudo pacman -Rsn  /// Удаление со всеми зависимастями 
git clone         /// Клонирует внешний git репозиторий /// Для AUR&GitHub и т.п.
makepkg -sriс     /// Сборка пакета из PKGBUILD'файла и его установка после сборки
cd                /// Переход в каталог по пути
ls                /// Вывод содержимого каталога
~~~
## Помощники для сборки
Далее мы с тобой будем использовать пакеты из [AUR](https://wiki.archlinux.org/title/Arch_User_Repository) или Arch User Repository, в нём содержаться пакеты написанные, как уже понятно из названия обычными пользователями. Сами пакеты представляют исходный код ввиде PKGBUILD файла для сборки на свой машине, а сборка производиться при помощи утилиты git, но ты можешь использовать "помощники AUR" для их сборки такие, как: yay, pamac (если у тебя Monjaro или ArcoLinux дистрибутив он будет уже установлен у тибя), pacaur, если у тебя ArchCraft то у тебя будет подключен сторонний репозиторий `[chaoit-aur]`. Я не имею ничего против них, но лучше производить установку "олдовым" методом через git, так как помощники могут устаревать или давать ошибки при сборке.
# Настройка Pacman 
Для начала мы настроим наш пакетный менеджер, что бы он мог работать сразу с несколькими пакетами сразу и получил к 32bit'ным зависимостям которые нам так же понадобятся.
Для этого введи комманду в терминале: 
~~~c+++
sudo nano /etc/pacman.conf
~~~
Убери `#` перед `multilib` и перед `include`, а также перед `ParallelDownloads` можешь свериться с скриншотами снизу

![pacman](https://sun9-84.userapi.com/impf/yeOtLO-qZHLGpL_vTYL2uNtxr-TNRh48nvytJg/XXlsNDAerCY.jpg?size=326x652&quality=96&sign=f0fb23686751d08f7e227dba85b0430b&type=album)
![pacman](https://sun9-49.userapi.com/impf/5WMtQxuvoXD9aUuz6gbF_1KOAQdWrOSCSH2qTg/zNK6snUIfRM.jpg?size=615x598&quality=96&sign=8410826b5c400721beaf61a64e7a8b40&type=album)


Далее просто обнови систему уже знакомой нам коммандой :
~~~c++
sudo pacman -Syyu
~~~
Так же сразу зададим параметр для сборщика пакетов, что бы он он мог производить компиляцию пакета в многопоточном режиме. Для этого перейдём в конфигурацию:
~~~
sudo nano /etc/makepkg.conf
~~~
И выставим нужные нам параметры к этим значения: 
~~~
MAKEFLAGS="-j$(nproc) -l$(nproc)"
RUSTFLAGS="-C opt-level=3"
~~~
Сделай как на скриншоте:

![make](https://sun9-43.userapi.com/impf/Uaacjz5FPptpay-hQ0IqUV2vScQJ7OZOHnxOZg/86DPjQKc6JY.jpg?size=618x305&quality=96&sign=d1ff6a7b297ab5cd6acbc8c902e01f58&type=album)

Далее мы ещё вернёмся к этому конфигу
___
Здесь важный момент, я подключаю сторонний репозиторий `[chaotic-aur]` так как считаю это удобным, он позволяет ставить некоторые уже собранные пакеты из `AUR`, но ты можешь им и не пользоваться собирая все пакеты компиляцией. Если у тебя ArchCraft то он уже подключен и его трогать не надо.

Комманды для поключения:
~~~c++
pacman-key --recv-key 3056513887B78AEB --keyserver keyserver.ubuntu.com
pacman-key --lsign-key 3056513887B78AEB
pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-keyring.pkg.tar.zst' 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-mirrorlist.pkg.tar.zst'
~~~

Не забудь добавить нужные строчкуи в `/etc/pacman.conf`, то что нужно добавить:
~~~
[chaotic-aur]
Include = /etc/pacman.d/chaotic-mirrorlist
~~~
Теперь обновись команндой:
~~~
sudo pacman -Syyu
~~~ 
После этого ты сможешь ставить предсобранные пакеты из AUR обычной командой установки.
# Базовые утилиты 
Теперь нам надо поставить базовые утилыты для работы с документами и архивами. Ты можешь поставить на своё усмотрение любые, но тут я укажу те которыми я пользуюсь:
~~~
sudo pacman -S git firefox p7zip unrar unzip lrzip file-roller ccache steam-native-runtime vim man links htop gedit vlc ranger libreoffice-fresh-ru ttf-jetbrains-mono ttf-liberation discord neofetch
__________________________________________________________________________________________________________________________________________________________________________________________________
git: поможет со сборкой пакет из AUR

Firefox: браузер, но ты можешь поставить chromuim, просто пропиши его вместо firefox

p7zip, unrar, unzip, lrzip, file-roller - помогут тебе с архивами

steam-native-runtime: Стим клиент.

vim: аналог стандартного nano, в будущем ты прохаваешь его фичастость, но сейчас он скорее всего будет сложноват для тебя, можешь не ставить

man, links: помошники синтекса и впринципе помогут быстрее набирать команды или заправишивать подсказки прямо в консоли к любой утилите.

gedit: так же текстовый редактор, но уже с полноценным интерфейсом, ставим на своё усмотрение.

vlc: музыка, ставим если нужно.

libreoffice-fresh-ru ttf-jetbrains-mono ttf-liberation: Офисный пакет Libreoffice сразу на русском языке, приятные шрифты для системы и офиса.  

ranger: аналог стандратного "проводника", но с псевдографическим интерфесом (терминальный), так же на свой усмотрение.

discord: ну и дискорд, думаю всем им пользуются.

neofetch: покажет все параметры нашей системы в консоли.
~~~
>Вернёмся к конфигу который мы редактировали ранее и включим ccache. Он нам нужен для увеличения скорости пересборки пакетов уже установленных в системе при обновленни. Перейдём в конфигурацию командой:
~~~
sudo nano /etc/makepkg.conf
~~~
И отредактируем строчку убрав `!` знак перед `ccache`: 
~~~ 
BUILDENV=(!distcc color ccache check !sign)
~~~
![make](https://sun9-32.userapi.com/impf/BeqhDRAjy1eUPlA-9lGYDclLxyvYyv2dpXrLOw/gWjUXUGJ9Vs.jpg?size=619x251&quality=96&sign=45e30ed444d8f20a8f44b9b0b4fd2a21&type=album)
>:exclamation: Иногда вызывает "карапшены" с при сборке ядра xanmod в obj_miss или повальном засыпании warning'ами. Если такое происходит вернуть `BUILDENV` в стандартное состояние
# Установка драйверов для видеокарты:
Одна из важнейших частей это драйвер видеокарты. Установка их так же давольно простая, но от них зависит производительность системы в играх.
## AMD видеокарты:
Тут всё просто, через пакетный менеджер ставим свободные MESA драйвера
~~~c+++
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader
~~~
Так же есть патченная версия TKG, но не на всех видеокартах стабильная, но более нацеленная на высокую призводительность в играх. Рекомендую протестировать. Поставится она сборкой через AUR или из репозитория  `[chaotic-aur]`. Для подколюченного репозитория ставится командой:
~~~
sudo pacman -S mesa-tkg-git
~~~
Так же рекомендую установить утилиту `CoreCtrl` для разгона или андервольтинга видеокарт AMD. Если вы подключили `[chaotic-aur]` установка производиться базовой командой:
~~~
sudo pacman -S corectrl
~~~
Если же не подключали `[chaotic-aur]` производим ручную сборку и установку пакета следующими командами:
~~~php
git clone https://aur.archlinux.org/corectrl.git
cd corectrl
makepkg -sric
~~~
>Что бы зафиксировать режим электропитания на "High" для видеокарт AMD рекомендую создать сервис, который в авторежиме при старте будет устанавливать правило питания на "High". Для этого создадим сервис:
~~~
sudo nano /etc/systemd/system/power_gpu.service
~~~
Далее прописываем в скрипт сервиса следующее:
~~~
[Unit] 
Description=power_gpu 
 
[Service] 
Type=oneshot 
ExecStart=/usr/bin/bash -c "echo high | sudo tee /sys/class/drm/card0/device/power_dpm_force_performance_level" 
RemainAfterExit=yes 
 
[Install] 
WantedBy=multi-user.target
~~~
Теперь просто активируюем данный скрипт в системе следующей командой:
~~~
sudo systemctl enable power_gpu
~~~
Теперь видеокарта не будет сбрасывать частоты. Для тех кто хочет заморочиться с более глубой настройкой электропитания видеокарты ссылка на [ArchWiki](https://wiki.archlinux.org/title/AMDGPU).
## Nvidia видеокарты:
Для видеокарт Nvidia драйвера ставятся командой:
~~~
sudo pacman -S nvidia-dkms nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader lib32-opencl-nvidia opencl-nvidia libxnvctrl
~~~
Для старых видеокарт Nvidia есть драйвер Nouveau и ставиться он вот этой командой:
~~~
sudo pacman -S mesa lib32-mesa xf86-video-nouveau vulkan-icd-loader lib32-vulkan-icd-loader
~~~
>Внимание! Так как у меня отсутствуе видеокарта Nvidia, по ним даны только базовые рекомендации, но по общественной информации могут возникать проблемы в сессии Wayland.
Воизбежание чёрного экрана в сессии Wayland рекомендуют добавить некоторые модули в конфигурацию ядра. Открыть конфигурацию ядра можно этой командой: 
~~~
sudo nano /etc/mkinitcpio.conf
~~~
Модули нужно добавлять в строчку `Modules=()`:
~~~
nvidia_drm nvidia_modeset nvidia_uvm nvidia
~~~
Строка должна принять такой вид:

![nvidia](https://sun9-37.userapi.com/impf/3h72pZNHYMHR_I46R-skBHCi98uQPJEzGdvkiQ/X_EsgWBUF8s.jpg?size=637x117&quality=96&sign=1764a73f2a786d625b04743316edfaa9&type=album)

Далее обновим кофигуряцию ядра командой:
~~~
sudo mkinitcpio -P
~~~
## Intel видеокарты:
Для них установка имеет аналогичный характер:
~~~
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader
~~~
## Микрокоды:
На данный момент Arch их ставит самостоятельно и ручной установки их не требуется, но вы можете проверить их наличие в системе и при отсутствии поставить вручную следующими командами:
~~~php
sudo pacman -S amd-ucode               /// Установить микрокод AMD
sudo pacman -S intel-ucode             /// Установить микрокод Intel
sudo mkinitcpio -P                     /// Обновление конфига
sudo grub-mkconfig -o /boot/grub/grub.cfg  /// Обновляем загрузчик ядра, который мы выбирали при установке.
~~~
# Установка звуковой подсистемы 
Здесь мы поговорим о звуке в Linux. На данный момент есть обычных звуковых сервера Alsa и PulseAudio, так есть Pipewire, но он выполняет функции не только звукого "драйвера"

Начнём с Alsa, это самый низкоуровневый звук идущий напрямую с ядра системы без обработки из-за чего имеет самую высокую "скорость" и наименьшие задержки. Что бы установить Alsa'у нужно установить эти пакеты:
~~~
sudo pacman -S alsa alsa-utils alsa-firmware alsa-card-profiles alsa-plugins alsa-lib
~~~
Главный минус `Als'ы` это настройка звука, что бы увеличить или уменьшить громкость в системе нужно заходить в псевдографическую обочку при помощи команды:
~~~
Alsamixer
~~~
Следующим вариантом является PulseAudio. Звуковой сервер который уже позволит нам настраивать звук в графической оболочке Pavucontrol. Что бы установить PulseAudio нужно установить следующие пакеты:
~~~
sudo pacman -S jack2 pulseaudio-alsa pulseaudio-jack pavucontrol jack2-dbus realtime-privileges
~~~
>Существую мнения о высокой задержке звука при использовании PulseAudio,
я могу подтвердить лишь "карапшены" звука на несколько мимиллисекунд только в сессии Wayland и только в паре игр, но возможно в сессии на x11 такой проблемы нету.

PipeWire самый последний из имеющихся звуковой сервер выполняющий функции не только звука. Если вы пользуетесь системой с сессией Wayland
то pipewire поможет вам так же в захвате изображения в том же OBS studio, а так же он не имеет проблем с задержкой как у PulseAudio. Для установки данного сервера нужны следующие пакеты:
~~~
sudo pacman -S jack2 pipewire pipewire-alsa pavucontrol pipewire-pulse alsa-utils wireplumber
~~~
Если вы записываете видио или стримите через OBS studi вам будут необходимы так же следующие пакеты:
Для чистого Wayland на базе WL-Roots (Sway WM и им подобные системы)
~~~
sudo pacman -S xdg-desktop-portal-wlr
~~~
Для систем с окружением KDE Plasma необходим будет следущий пакет:
~~~
sudo pacman -S xdg-desktop-portal-kde
~~~
Для систем с окружением GNOME необходим : 
~~~
sudo pacman -S xdg-desktop-portal-gnome
~~~
Для пользователей окружения на сессии x11 XFCE и им подобным будет необходим:
~~~
sudo pacman -S xdg-desktop-portal-gtk
~~~
Установив нужный "портальный" пакет вы сможете при помощи Pipewire захватывать экран в OBS studio или же другой программе для захвата экрана.
# Оптимизация файловой подсистемы и полезные сервисы
В системе Arch имеются две самых распространенных файловых системы - ETX4 и BTRFS.
ETX4 - простая как топор, универсальна для любых насителей, как SSD так и HDD, из-за чего хоть и эффективна, но нуждается в доработках.
BTRFS - одна из последних имеющихся файловых, так же проста, но имеет функциюю создания snapshots btrfs, позваляющая откатываться после неудачной настройки системы, а так же по стандарту запущенный TRIM таймер для накопителя.
## Оптимальные настройки носителя на примере BTRFS
Для найстройки параметров монтирования носителя перейдём в следующий конфиг командой:
~~~
sudo nano /etc/fstab
~~~
Для SSD носителей рекомендую такие параметры:
~~~c++
noatime,compress=zstd:3,ssd,ssd_spread,max_inline=256,space_cache=v2
~~~
Параметр `realtime` отвечает за запоминание времени доступа к файлам в системе и его вы можете замените на два альтернативных: lazytime и noatime. `noatime` - отключает запоминание последнее время доступа к файлу,а `lazytime` - производит дополнительную промежуточную запись в ОЗУ. При всех 3 параметрах проблем не наблюдалось, но на с lazytime возможно сильное увеличение потребления оперативной памяти.

Параметр `compress` отвечает за сжатие файлов на накопителе и позволяет экономить место на жётском диске. Рекомендую `zstd` алгоритм с базовым значением сжатия в `3` раза, но возможны варианты от `1` до `15`.
![fstab](https://sun9-87.userapi.com/impf/5uQdyPNdDaksTFmGjhEpzXoTt_4bvvSGU1hxFA/vZXf9MPGONc.jpg?size=1055x106&quality=96&sign=21fa970044595d970813907340ca8c96&type=album)
## Полезные сервисы для системы
Zram - это модуль которы создаст в ОЗУ сжатое блочное устройство. Простыми словами это RAM-диск со сжатием данных на лету, которое может использоваться системой, как временное хранилище данных или файлом подкачки. Изначально выключим swap файл в системе следущей командой:
~~~
sudo swapoff -a      /// Выключается swap в системе
sudo rm -f /swapfile /// Удаляет все файлы подкачки в системе
sudo nano /etc/fstab /// Если имеется строчка с флагом монтирования swap 
необходимо удалить её
~~~
Так как сейчас Zram это базовый модуль в системе его просто необходимо просто активировать и настроить. Первым делом перейдём в режим суперпользователя командой:
~~~
sudo su
~~~
Теперь добавим Zram модуль в загрузку системы, для этого зайдём в конфигурационный файл Zram и просто пропишем туда - `zram`:
~~~c++
nano /etc/modules-load.d/zram.conf
вставить: zram
~~~
Настроим количество модулей:
~~~c++
nano /etc/modprobe.d/zram.conf
вставляем данный параметр: options zram num_devices=1
~~~
Теперь нам необходимо настроить правила для Zram модуля. Перейдём в Rules файл и внесём туда нужные параметр:
~~~c++
nano /etc/udev/rules.d/99-zram.rules
~~~
В rules файле пропишите данную строку:
~~~c++
KERNEL=="zram0", ATTR{disksize}="512Mb" RUN="/usr/bin/mkswap /dev/zram0", TAG+="systemd"
~~~
Вместо `512Mb` укажите нужное количество ОЗУ которое вы готовы отдать под Zram, рекомендую указывать `40%` от вашей ОЗУ в `Mb`.

Финально примонтируем данный раздел к системе, для этого вернёмся в конфигурационный файл файловой системы:
~~~c++
nano /etc/fstab
~~~
В пропишем строчку:
~~~
/dev/zram0 none swap defaults 0 0
~~~
Так же установим ещё несколько модулей: 

`dbus-broker`: данный сервис является заменной libdbus для нашего systemd, цель которой обеспечить высокую производительность и надежность при сохранении совместимости с эталонной реализацией D-Bus, фактически её задача увеличить скорость общаения системой с PCIe портами. Уставнока производиться следующими командами:
~~~
sudo pacman -S dbus-broker
sudo systemctl --global enable dbus-broker.service
~~~
`rng-tools`: сервис, следящий за энтропией системы через аппаратный таймер. Может сильно сократить длительность загрузки системы. Устанавливается следующими командами:
~~~
sudo pacman -S rng-tools         
sudo systemctl enable --now rngd 
~~~
Для ETX4 файловой системы рекомендую запустить сервис TRIM следующей командой:
~~~
sudo systemctl enable fstrim.timer
ручной запуск TRIM: sudo fstrim -va
~~~
`Ananicy CPP`: сервис распределяющий приоритеты задач в системе, чем сильно повышает отклик в системе. Установка из `[chaotic-aur]`:
~~~
sudo pacman -S ananicy-cpp
sudo systemctl enable --now ananicy-cpp
~~~
установка правил для распределения:
~~~
sudo pacman -S ananicy-rules-git
sudo systemctl restart ananicy-cpp  
~~~
Для ручной установки из AUR репозитория:
~~~
git clone https://aur.archlinux.org/ananicy-cpp.git 
cd ananicy-cpp                                      
makepkg -sric                                       
sudo systemctl enable --now ananicy-cpp 
~~~
установка правил для распределения:
~~~
git clone https://aur.archlinux.org/ananicy-rules-git.git 
cd ananicy-rules-git                                     
makepkg -sric
sudo systemctl restart ananicy-cpp
~~~
# Электропитание
Стандартно процессор динамически меняет свою частоту сохраняя баланс между потреблением и скоростью. Так как нам необходимо получить хорошую производительность в системе переведём план электропитания в более агрессивный. Для этого установим модуль управления питанием `cpupower`
командой:
~~~
sudo pacman -S cpupower
~~~
Далее редактируем в конфигурационном файле строчку уберая `#` перед governor и ставим высталяем после `=` значение ’performance’. Ставим модуль в автозагрузку командой:
~~~
sudo nano /etc/default/cpupower 
sudo systemctl enable cpupower /// Запуск службы
~~~
![cpu](https://sun9-59.userapi.com/impf/KWtfEIfhJpKcYKcLL4Esn9D9ZJ1CAFCdbySaCQ/-yKemDTonzk.jpg?size=684x106&quality=96&sign=5c9e1193ed510f05ecc44f4b29204fa8&type=album)

Так же рекомендую выключить гибернацию и спящий режим в системе. Для этого ставим `Polkit`:
~~~
sudo pacman -S polkit
~~~
Редактируем конфиг переходя в него:
~~~
sudo nano /etc/polkit-1/rules.d/10-disable-suspend.rules 
~~~
И прописываем в него эти строчки:
~~~php
polkit.addRule(function(action, subject) {
  if (action.id == "org.freedesktop.login1.suspend" ||
      action.id == "org.freedesktop.login1.suspend-multiple-sessions" ||
      action.id == "org.freedesktop.login1.hibernate" ||
      action.id == "org.freedesktop.login1.hibernate-multiple-sessions")
  {
      return polkit.Result.NO;
  }
});
~~~
# Отключение системных заплаток в ядре
В стандартном исполнении в системе имеются заплатки безопасности spectre и meldown. Я рекомендую их отключть, для этого мы перейдём в конфигурационный файл ядра:
~~~
sudo nano /etc/default/grub
~~~
![grub](https://sun9-18.userapi.com/impf/yRQAxpv1uHeIP5nHqd0FcOynTeYIT2hs6-QDTQ/yGmTwgtjObA.jpg?size=1280x119&quality=96&sign=30b5f921f3a789b21ffd03e3da801f59&type=album)
В строчку `GRUB_CMDLINE_LINUX_DEFAULT=` прописываем следующие параметры
~~~
mitigations=off noibrs nowatchdog preempt=none raid=noautodetect rootfstype=btrfs elevator=noop noibpb nopti nospectre_v2 nospectre_v1 l1tf=off 
~~~
+ elevator=noop: указывает для всех дисков планировщик ввода NONE. **Не использовать если у вас жесткий диск.** 

+ Так же уберите строчку raid=noautodetect, если используете RAID массивы. 

+ Для видоекарт AMD рекомендую так же добавить параметр маски указанный ниже, который нужен для модуля CoreCtrl, разблокирующий нам андервольтинг GPU, а так же на некоторых видеокартах позволит разгонять VRAM на которых небыло такой опции по задумке производителя, так же пофиксит вылеты системы на нестандартных (кастомных) прошивках видеокарты. 
~~~
amdgpu.ppfeaturemask=0xffffffff
~~~
Далее произведём обновление параметры `GRUB` загрузчика системы командой:
~~~
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
## На данном этапе рекомендую произвести перезагрузку системы
# Кастомные Linux Ядра
Здесь я как можно коротко постараюсь рассказать о кастомных ядрах для линукс. На данный момент существует несколько самых популярных кастом ядер для linux: Linux-Zen, Linux-liquorix, Linux-Xanmod, Linux-TKG. Все эти ядра имеют разную производительность на разных машинах, если вы хотите получить максимум от свой системы то стоит производить установку именно скомпилированного ядра на своей машине под своё железо. Так же желательно установить все 3 ядра и произвести тесты производительности на каждом, но не стоит надеятся на высокий уровень прироста производительности. Данный этап является минорным и позволяет "дожать" лишние 5-15 кадров в играх.
Так же если вы хотите производить компиляцию на своёй машине рекомендую использовать стандартное Arch ядро или Lts его версию для возможности отката в базовые параметры.
>:exclamation:Важная ремарка, сборка ядра на своей машине может занимать ОЧЕНЬ МНОГО ВРЕМЕНИ, от 2 до 8 часов в зависимости от мощности железа. 

Теперь давайте выберем Ядро :
## Zen Ядро
Zen ядро - это можно сказать около стоковое ядро, имеющее не большое количество патчей из опыта домашних пользователей, что делает его стабильным, но не самым быстрым из всех. Основные изменения коснулись:
+ Улучшение планировщика CFS, что должно снизить задержки в системе посравнению со стандартным ядром.
+ Добавлен BFQ планировщие, имеющий пониженные задержки на I/O за счёт большей пропускной способности.

Главное его приемущество это наличие его в оффициальном репозитории, а значит установка бинарной его версии просто производиться обычной командой:
~~~
sudo pacman -S linux-zen linux-zen-headers
~~~
Далее обновляем загрузчик командой:
~~~
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
Для тех кто хочет скомпилировать ядро под свою машину:
~~~
git clone https://aur.archlinux.org/linux-zen-git.git
cd linux-zen-git
makepkg -sric
~~~
Не забываем обновить загрузчик:
~~~
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
## Xanmod ядро
Альтернативное ядро в которое уже было добавлено большее количество патчей под игрушки, но само ядро более нацелено на железо `AMD`, но может быть использовано и на на системах `Intel` хотя ранее были случаи проблем с дросселированием частот процессора (будте осторожны!). Основными улучшениями тут стали:
+ Наличие WineSync синхронизации являющейся аналогом Fsync, но вынесенного отдельным модулем, так же в последних EDGE версиях ядро поддерживает WineSync_Futex2, что полезно для многоядерных "камней"
+ Множество патчей с которыми вы сможете ознакомиться на GitHub его создателя: [Xanmod](https://github.com/xanmod/linux-patches)

Установка его возможна из `[chaotic-aur]` репозитория базовыми командами:
~~~
sudo pacman -S linux-xanmod-edge linux-xanmod-edge-headers
~~~
Или же его нативная компиляция из AUR репозитория:
~~~
git clone https://aur.archlinux.org/linux-xanmod-edge.git                  
cd linux-xanmod-edge
export _makenconfig=y _use_numa=n use_tracers=n
makepkg -sric
~~~
Если при сборке ядра поялвяется ошибка PGP-ключей необходимо последнюю команду произвести в этом виде:
~~~
makepkg -sric --skippgpcheck
~~~
Так же после установки не забываем обновить загрузчик:
~~~
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
## liquorix ядро
linux-liquorix - Debian ядро, которое по своей сути является аналогом `Zen` ядра, но с чуть большей направленностью в игровую стезю. Главные их различия:
+ Базово предустановленный PDS планировщик (Возможность выбора BMQ и CFS палнировщиков оставлена), который .
+ Увеличенная частота системного таймера до 1000Mhz, которая в теории должна дать чуть большую плавность в играх.
+ Так же ссылочка для ознакомления со всеми патчами этого ядра [Liquorix](https://liquorix.net/)

Так же, как и у Xanmod ядра установка liquorix ядра возможна из `[chaotic-aur]` репозитория базовыми командами:
~~~
sudo pacman -S linux-lqx linux-lqx-headers
~~~
А для всех желающих возможна нативная сборка ядра из AUR репозитория:
~~~
git clone https://aur.archlinux.org/linux-lqx.git
cd linux-lqx 
sed -i 's/_makenconfig=/_makenconfig=y/' PKGBUILD
makepkg -sric
~~~
Так же возможны проблемы с PGP-ключами, в этом случае производим компиляцию с установкой данной командой:
~~~
makepkg -sric --skippgpcheck
~~~
## Linux-TKG ядро
Linux-TKG - это окрошка или же конструктор из всего, что есть на данный момент. Данное ядро рекордсмен по патчам для игровой производительности начианя от планировщиков для CPU заканчивая патчами под определённые игры.
Так же ядро имеет, как и остальные ядра два вида установки, предсобранная версия из `[chaotic-aur]` репозитория, с разными видами планировщиков (В моём случае PDS оказался лучшим) при помощи команд:
~~~
sudo pacman -S linux-tkg-pds
~~~
Или же методом компиляции под своё железо, производить настройку ядра будем редактированием конфиг файла компиляции:
~~~
git clone https://github.com/Frogging-Family/linux-tkg.git
cd linux-tkg
nano customization.cfg
~~~
Эти патчи я рекомендую к активации:
+ _menuconfig="2" - даст возможно настроить ядро через меню, как `xanmod` и `liquorix`.
+ _cpusched="pds" - выбираем необходимый нам планировщик, мне понравился pds планировщик, но вы можете попробова так же cacule планировщик, который использутся в deb пакете `xanmod` ядра.
+ _tickless="1" - фиксирует таймер на одной частоте.
+ _timer_freq="1000" - задает частоту таймера в системе. 
+ _fsync="true" - активирует Fsync патч.
+ _futex2="true" - активирует Futex2 патч.
+ _zenify="true" - использование патчей `Zen` и `Liquorix` ядер, что должно дать буст в играх.
Теперь собираем и устанавливаем ядро:
~~~
makepkg -sric
~~~
И так же не забываем обновить `GRUB` загрузчик:
~~~
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
## Настройка ядра при компилияции на своей машине
Тут я покажу те параметры, которы стоит менять в ядре при его нативной компиляции. Эти параметры будут актульны для всех ядер одинаковы, но вы можете использовать любые другие поправки в них.

Самое важное это выбор вашего процессора, что бы ядро смогло подорбрать конфигурацию к нему. Настройка через это меню эдентична для `Xanmod` `Liquorix` и `Linux-TKG` ядер. Далее следуйте скриншотам:

![первый скрин](https://sun9-60.userapi.com/impf/RwQaMgYYimtwJL3PiM3iwLOD_fDScd-03XXWcw/bF27WZkplCw.jpg?size=469x451&quality=96&sign=9277dcafc3fcba22afc59e6e0320e0b2&type=album)
![второй скрин](https://sun9-72.userapi.com/impf/MGnTRGiKiAXzrYBX0oEDYbfxNl6y22Rga4Zt9Q/rUZlXONGgkM.jpg?size=658x438&quality=96&sign=8a425d0b055e7b8b7e3b1a10efef12c3&type=album)
### Здесь выбиарем семейство своего процессора или же просто AMD-native, Intel-native и компилятор сам распознает все параметры:
![Третий скрин](https://sun9-15.userapi.com/impf/YWCG1-6y_FYEblctff9MrlfEXzIaPGfYJcC1tA/8osHYZcgrq8.jpg?size=519x826&quality=96&sign=cf8efb29ea07b2ab799723a9f9918f31&type=album)

### Далее давайте настроим системный таймер в ядре:
![первый скрин](https://sun9-85.userapi.com/impf/gz9I46tcPbiT0jkI9cJy-dF29cGZ2KQYffQIJw/smnXdjET_HE.jpg?size=474x435&quality=96&sign=986436fe96a8846ec32477ee6823bf67&type=album)
![второй скрин](https://sun9-67.userapi.com/impf/zkSArGAFCszBdbE9wMQDK2ngg9FWSsc_uNnQOg/wqx-pBLi6VE.jpg?size=666x391&quality=96&sign=cdfcabe8da1fa0f437b19a8ec044566b&type=album)
![третий скрин](https://sun9-54.userapi.com/impf/3nY6akTpFvFf8EE-TPxnh60y8Wod9pXIy3kgSQ/tTy4l5f9eJE.jpg?size=627x200&quality=96&sign=2825b82bea7291080bd54610633bb96b&type=album)
### Здесь нам предлагают выбрать тип таймера, где:
+ Periodic timer ticks - осуществление тика статически через частоту.
+ dle dynticks system - прерывания через частоту тика N только тогда, когда процессор чем-то занят.
+ Full dynticks system - прерывания через частоту тика N, но не всегда, даже если процессор чем-то занят.

![4 скрин](https://sun9-25.userapi.com/impf/iBZpKmuMsLEI-HVOKapjbpKHIsxmE0kjk26R5g/lToYIvWTF08.jpg?size=530x165&quality=96&sign=5cd1e560c3d2909495c2405dcf02d965&type=album)
### Теперь давайте выставим нужную частоту таймера:
![скрин](https://sun9-60.userapi.com/impf/RwQaMgYYimtwJL3PiM3iwLOD_fDScd-03XXWcw/bF27WZkplCw.jpg?size=469x451&quality=96&sign=9277dcafc3fcba22afc59e6e0320e0b2&type=album)

![скрин](https://sun9-60.userapi.com/impf/3f6EbNXFVyd0z7VKxSIXJ8iP19mDto9D6IEveA/eWYM2Qe09L0.jpg?size=627x369&quality=96&sign=b36b63b66e6943c37a0b86d011c14910&type=album)

![скрин](https://sun9-12.userapi.com/impf/8-6mSkIRSa5rmG5O8CqV4pI-i5B8S3s4fJ-nLQ/pEXHgNQWN7I.jpg?size=446x204&quality=96&sign=a2ac7451b9bdd3a222df7004e5926605&type=album)

Далее просто жмём `F9` или `ESC` до вопроса о сохранение, соглашаемся на сохаранение и установка кастомного ядра. Далее будет происходить сборка ядар и вот она уже может займять много времени о чём я указывал ранее.
# Запуск Игр и Windows приложений на Linux
Самое время рассказать про запуск игр в Linux системах. Для запуска приложений Windows в Linux используется прослойка Wine/Proton, а так же производится ретрансляторы DirectX сигналов в понятные для Linux API - Vulkan OpenGL и OpenMAX. Для этого нам понадобятся слебующие модули: Wine, DXVK, VKd3d.
## Wine
Как уже было сказано выше в Linux используется Wine прослойка для запуска .exe приложений. На данный момент существуют несколько вариаций: 
+ Wine стабильная версия
+ Wine-staging самая свежая версия и имеющая нестабильные модули
+ Wine-TKG-staging-Fsync версия содержащая Fsync патч, а так же патчи от создателя одноименного ядра TKG

Для установки базовой Wine и его зависимойсте выполните следующую команду:
~~~
sudo pacman -S wine winetricks wine-mono giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader
~~~
Для установки staging версии Wine и его зависимойсте выполните следующую команду:
~~~
sudo pacman -S wine-staging winetricks wine-mono giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader
~~~
Версия TKG так же может быть установлена из репозитория `[chaotic-aur]` или собрана из репозитория [GitHub](https://github.com/Frogging-Family/wine-tkg-git) создателя. Покажу на примере установки из `[chaotic-aur]`:
~~~
sudo pacman -S wine-tkg-staging-fsync-git
~~~
## DXVK
DXVK это метод ретрансляции DirectX в Vulkan API. Он нам понадобиться для нативного запуска приложений/игр использующих Directx версий 9,10 или 11.
Я рекомендую вам устанавливать версию с ASYNC патчем, который позволяет производить компиляцию асинхронных шейдеров. Установить `DXVK_ASYNC` можно так же из AUR или же из репозитория `[chaotic-aur]`. Сборка из AUR производиться следующими командами:
~~~
git clone https://aur.archlinux.org/dxvk-mingw.git
cd dxvk-mingw
makepkg -sric
~~~
Из `[chaotic-aur]` репозитория устанавливается следующей командой:
~~~
sudo pacman -S dxvk-mingw
~~~ 
Далее в параметрах запуска игр необходимо будет добавить следующую команду:
~~~
DXVK_ASYNC=1 %command%
~~~
## Proton
Протое это прослойка придуманная Valve для запуска игр под Linux. Существует на данный момент две ветки Proton:
+ Proton - встроенный в Steam, переодически обновляется и добавляются новые патчи
+ Proton-ge-custom - так же часто обновляется, но в него интергруются патчи от GloriousEggroll для увеличения производительности, а так же имеет встроенный AMD FSR, который можно активировать в любой игре.

Я рекомендую ставить версию от  GloriousEggroll так как она чаще даёт больше FPS в играх. Устанавливать её так же можно AUR или GitHub репозитория [GloriousEggroll](https://github.com/GloriousEggroll/proton-ge-custom/releases), ну так же из `[chaotic-aur]`. Покажу на примере установки из AUR:
~~~
git clone https://aur.archlinux.org/proton-ge-custom.git
cd proton-ge-custom
makepkg -sric
~~~
Что бы активировать AMD FSR необходимо в параметрах запуска прописать следущее:
~~~
WINE_FULLSCREEN_FSR=1 WINE_FULLSCREEN_FSR_STRENGTH=1 %command%
~~~
>WINE_FULLSCREEN_FSR_STRENGTH= отвечает за параметр резкости в режимах от 0-5, где 0 - самая высокая резкость, а 5 - минимальную. 
## VKD3D
Данный ретранслятор понадобиться нам для приложений и игр работающих на базе DirectX 12. После установки там где он нужен, поумолчанию будет активирован будь то Steam или же Bottles. Установить его можно так же из AUR или `[chaotic-aur]` репозитория. Установка из AUR выполняется такими командами:
~~~
git clone https://aur.archlinux.org/vkd3d-proton-mingw.git
cd vkd3d-proton-mingw 
makepkg -sric
~~~
Теперь у нас есть всё, что бы запускать Windows игры и приложения в Linux системе.
# Запуск игр и приложений в Linux системе
После всех прошлых шагов мы находимся на финишной прямой. У нас есть Steam, но можно поставить ешё спецальный игровой режим, который добавит ещё парочку FPS, устанавливается он просто:
~~~
sudo pacman -S gamemode lib32-gamemode
~~~
Так же давайте установим мониторинг FPS для игр. В Linux он называется **`Mangohud`** и устанавливается он из AUR или же `[chaotic-aur]` :
~~~
git clone https://aur.archlinux.org/mangohud.git    
cd mangohud                                          
makepkg -sric
~~~
![mango](https://sun9-63.userapi.com/impf/zYnEFUuehKZnRarLIcS0xiI3f9ISZfnGax58Ng/Fy6Trx9_WaA.jpg?size=315x472&quality=96&sign=ec9d5b24db02809294c8ffbcbad47c26&type=album)

И установим графический помошник для MangoHud'a, ****`GOverlay`****, этой командой:
~~~
git clone https://aur.archlinux.org/goverlay.git 
cd goverlay                                      
makepkg -sric  
~~~
Выглядит он так:

![Goverlay](https://sun9-2.userapi.com/impf/k8aGryEleCDk_jrD6gMYWn2er0b4wi4xR9R1ZQ/NkE_JfqAoX8.jpg?size=1255x630&quality=96&sign=719c1ae9b2d99f6ce6740f3096ab9489&type=album)
В неё мы сможешь устанавливать FPS Lock для игр и включать и выключать VSYNC

Аналогом ReShade или Nvidia Filters является **`VKBASALT`** - он так же может накладывать различные фильтры на игры и уже в базе имеет свой набор филтров. Установка возможна, как из AUR так и из `[chaotic-aur]` репозитория.
Установка из AUR выглядит так:
~~~
git clone https://aur.archlinux.org/vkbasalt.git
cd vkbasalt
makepkg -sric
~~~ 
А сам эффекты настраиваются через меню GOverlay:

![vkbasalt](https://sun9-3.userapi.com/impf/tqRMp3ez0hRAbOszep_KIyA0gICXu7DzD-39tg/zRJ_iatmMrI.jpg?size=1246x612&quality=96&sign=72993073db5a33ee27c624ccfa9627c0&type=album)

## Steam
Для запуска сторонних игр через **`Steam`** нужно просто нажать добавить игру -> стороннюю игру и выбрать exe файл от игры, а в свойствах выбрать самой игры выбираем Proton-GE. Далее жмём играть и запустится установщик.
Вот пример установка игры, которой нету в Steam:

![1](https://sun9-70.userapi.com/impf/E_MWfWNJX1Vnkzjm1ZuoQYO4WJR0GT0AcPvMKA/BUXBwv1cOYk.jpg?size=286x177&quality=96&sign=f71ee8bfb52398a0a4ddb1748503701d&type=album)
![2](https://sun9-86.userapi.com/impf/0sjBhZ8adzi3NrxRn-P14Jung3NbuDf1VtDDBg/oXURWsFKBow.jpg?size=934x329&quality=96&sign=b359d744837d9a84208dffed478914bd&type=album)
![3](https://sun9-19.userapi.com/impf/Gt09HIiJlhIxvNvAssTVUoFI3oMoWsMtrgCeaA/dHzPxsqeovg.jpg?size=635x224&quality=96&sign=b4cf07b294a9862fcd9d24294ed5f331&type=album)
![4](https://sun9-67.userapi.com/impf/tVNrKiUAmz4M0iXwAF3WWKb5qc4XaCioxv7vrQ/OAPJkOKtzxw.jpg?size=1280x723&quality=96&sign=ecc8c716dddb0a14aec358ec280e6542&type=album)
### Обычно я использую для игр эти параметры запуска: 
~~~
WINE_FULLSCREEN_FSR=1   mangohud gamemoderun DXVK_ASYNC=1 WINE_FULLSCREEN_FSR_STRENGTH=2 ENABLE_VKBASALT=1 %command%
~~~
## Bottles 
Она так же позволяет устанавливать игры и приложения, является аналогом Lutris и имеет схожий инструментарий. Его я могу рекомендовать когда вы разберётесь в системе глубже, но я покажу как базово запустить игру через данный Лаунчер. Следуйте скринам и всё получится:
+ Нажимаем **`+`** в левом углу и выбираем, что будем устанавливать 

![1](https://sun9-88.userapi.com/impf/9H58n7to5ucfMuwJiB7JF0HxDKaLgjfyoFtLEA/iGVzt1bTi8M.jpg?size=172x156&quality=96&sign=d80631c1acd428c3ad1b6259f356de05&type=album)
![2](https://sun9-23.userapi.com/impf/vpII0yMSZusNVetg9PSaUquHsXnyz4p2DzZGAg/APK2hJL8Kio.jpg?size=493x440&quality=96&sign=bc41eae5fa449208b5cbb6cc02402062&type=album)
+ Все опции здесь можно включать ползунками без введения команд запуска, но и отдельно вы тоже можете их ввести

![3](https://sun9-88.userapi.com/impf/SEH6_VxhYRzzx7HxskxWA-Jqg6EMaoqa0IkGVQ/V7U7pzLAVHM.jpg?size=856x612&quality=96&sign=67d8ba2a4c1b07a879d9b73014f4c86a&type=album)

+ Далее эмём `run executable` и выбираем .exe файл игры или её установщика и жмём `run` после чего запустится игра/приложение

![4](https://sun9-51.userapi.com/impf/wHFhIprNgeGFIydSvYLssGtmgQ7NQkNZe-b2yA/Lc9AMkvkvYI.jpg?size=597x57&quality=96&sign=1520fffe025f94e31943bad9a1c061da&type=album)

![5](https://sun9-80.userapi.com/impf/OJDUy6rheQDyovHq958-ajjHKluC9PBuOqJE0w/Yw1E5diRkQ4.jpg?size=988x200&quality=96&sign=3336f4363e25b81b9d2fcea172f8dcaf&type=album)
# На этом базовую настройку и конфигурирование ArchLinux системы можно считать оконченной. Система может запускать любые игры и приложения, а вы ознокомились со всем теми базами, что вам помогут в дальнейшей эксплуатации системы.
