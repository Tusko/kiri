# Можливості

[English](features.md) · **Українська**

Що Kiri підтримує зараз через Docker-контекст `kiri`.

## Платформи

Mac з Apple silicon та Intel (macOS 14+), Windows 10 і 11 (x86-64 та Arm, через WSL 2) і Linux (.deb та .rpm, перевірено на Ubuntu й Fedora).

## Що працює

- **Контейнери**: `run` (`--rm`, `-d`, `-it`, `-e`, `-u`, `-w`, `--name`,
  `--restart`, `--entrypoint`), `exec`, `logs`, `attach`, `stop`, `kill`,
  `restart`, `rm`, `wait`, `rename`, `pause`/`unpause`, `update`, `stats`,
  `top`, `diff`, `export`, `commit`, `cp` (в обидва боки, для запущеного й
  зупиненого контейнера), `container prune`; записи переживають перезапуск
  демона; фільтри в кожному списку
- **Обмеження й безпека**: `--memory`, `--cpus`, `--cpu-shares`,
  `--pids-limit`, `--shm-size`; типові capabilities Docker і типовий
  seccomp-фільтр; `--cap-add`/`--cap-drop`, `--privileged`, `--read-only`,
  `--security-opt seccomp=unconfined` і `no-new-privileges`. Навіть
  привілейовані контейнери й контейнери без обмежень не виходять за межу
  хоста: без сокетів vsock (шлях до Mac) і без io_uring
- **Мережа**: типовий bridge, власні мережі з DNS за іменем і псевдонімом,
  `network connect`/`disconnect` (як і в Docker, контейнер може вийти з
  будь-якої своєї мережі, зокрема з першої: тоді типовий маршрут дає інша
  його мережа, а без жодної він лишається без мережі, як із
  `--network none`; щоб вийти з першої мережі, запущений контейнер спершу
  зупиняється), `--network host|none`, `-p`/`-P` (TCP і UDP) на всіх
  інтерфейсах (як у Docker; `-p 127.0.0.1:8080:80` лишає порт локальним),
  `--add-host`, `host.docker.internal`, `--network container:<name>`
  (у Compose `network_mode: service:x`; як і в Docker, якщо цей контейнер
  перезапуститься, контейнери з його мережею лишаються в старому, уже
  порожньому просторі імен, доки їх теж не перезапустити), IPAM (`--subnet`,
  `--gateway`, `--ip-range`, `--aux-address`), `--ip`, `--mac-address`,
  `--dns`, `--dns-search`, `--dns-option`, `--domainname`, `--link`,
  `--ipv6` (у Compose `enable_ipv6`: підмережа IPv6 поруч з IPv4, із
  `--subnet` або власна /64 Kiri в межах fd00::/8; типовий bridge лишається
  лише IPv4, як і в Docker), а також `http://<name>.kiri.local`, щойно ви це
  ввімкнете (застосунок запитує під час першого запуску; змінити можна в
  «Налаштування» → «Домени»), або `http://<name>.kiri.localhost` без
  налаштування. Сервіси Compose відповідають на
  `<service>.<project>.kiri.local`, а мітка `dev.kiri.domain=api.myapp`
  задає контейнеру власні імена. HTTPS теж є (`https://<name>.kiri.local`):
  власний центр сертифікації Kiri лежить у `~/.kiri/tls`, і його обмеження
  імен дозволяють лише `*.kiri.local` та `*.kiri.localhost`
  («Налаштування» → «Домени» → HTTPS просить macOS довіряти йому). Порти
  маршрутизаторів із термінала: `kiri domains` показує їх,
  `kiri domains set --http 8080 --https 8443` змінює без перезапуску,
  `kiri domains reset` повертає 80 і 443
- **Сховище**: іменовані й анонімні томи, монтування шляхів Mac (virtiofs),
  `--tmpfs`, `--mount`, команди `volume`
- **Образи**: `pull` (з обліковими даними `docker login`), `push`, `tag`,
  `save`, `load`, `history`, `search`, `image prune`, `system df`;
  **образи amd64 працюють через Rosetta**, `--platform` враховується
- **Збірки**: `docker build` / `buildx build` через BuildKit, що працює в
  контейнері Kiri (драйвер docker-container у buildx) і зупиняється під час
  простою; на Apple silicon кроки збірки для amd64 теж виконуються через
  Rosetta
- **Compose**: `up`, `down`, `ps`, `logs`, `exec`, кілька мереж, події
- **USB-пристрої (бета)**: вкладка USB передає пристрій (послідовний
  адаптер, Zigbee-стик, плату для розробки) вашим контейнерам на Mac,
  Windows і Linux та показує, де він з’явився в Linux, разом із рядком
  `--device` для копіювання. Якщо на Mac під’єднаний пристрій зникає з шини
  (плата перезавантажується або Mac засинає), Kiri під’єднує його знову,
  коли він повертається
- **Інтеграція з Mac**: файли контейнерів у `~/Kiri` у Finder (NFS лише
  для читання, завжди актуальні), переадресація SSH-агента в
  `/run/host-services/ssh-auth.sock`
- **Пам’ять**: balloon повертає невикористану пам’ять під час роботи, а ВМ
  засинає після 60 с простою (демон займає ~15 МБ; пробудження триває
  ~0,15 с)
- **Вікно**: контейнери, образи, томи й мережі, проєкти Compose згруповано
  (запуск, зупинка, перезапуск чи видалення всього проєкту); у кожного
  контейнера своя сторінка з подробицями, журналами наживо (пошук, фільтр
  за рівнем), командним рядком («Відкрити в Терміналі» для повної оболонки)
  і файлами (перегляд, «Зберегти в Завантаження», «Показати у Finder»);
  налаштування шрифтів, розміру тексту, ЦП і пам’яті ВМ (`~/.kiri/vm.json`,
  діє з наступного запуску ВМ; `KIRI_CPUS` і `KIRI_MEMORY_MIB` мають
  пріоритет) і локальних імен
- **Рядок меню**: коли вікно закрито, Kiri лишається в рядку меню зі станом
  ВМ, запущеними контейнерами (клацніть, щоб відкрити) і кнопками
  запуску й зупинки
- **Kubernetes**: `kiri k8s start` запускає одновузловий кластер k3s в
  одному контейнері, готовий за ~20 с, як kubectl-контекст `kiri`
- **Linux-машини**: `kiri machine create dev --distro ubuntu` і
  `kiri machine shell dev`: ваш користувач, sudo і `/Users` за тим самим
  шляхом
- **Застосунок**: `cargo xtask app` збирає `Kiri.app` з CLI `docker`,
  Compose і buildx всередині; під час відкриття він виконує `kiri setup`
  (Docker-контекст, LaunchAgent, збирач) і відкриває вікно; `kiri update`
  встановлює нові версії, перевіривши їхні SHA-256 і підпис
- **Linux-хости**: той самий `kiri` без ВМ, агент працює на ядрі хоста
  (`cargo xtask linux`, `contrib/linux/kiri.service`); arm64 і x86-64;
  `tests/linux-host.sh` це перевіряє. Пакети `.deb` і `.rpm`
  (`packaging/linux`): `kiri` з CLI docker, Compose і buildx або
  `kiri-lite` лише з рушієм. Членство в групі `kiri` рівнозначне правам
  root, так само як і в групі `docker`
- **Windows**: Linux-збірка x86-64 у дистрибутиві WSL2 і `kiri-win.exe`,
  який обслуговує API на `npipe:////./pipe/kiri` (Docker-контекст `kiri`) і
  передає його у WSL через один `wsl.exe … kiri bridge`, тож мережеві
  налаштування WSL і Windows змінювати не треба (`cargo xtask msi` збирає
  `target/Kiri-<version>-x86_64.msi`, інсталятор для одного користувача
  на основі `target/kiri-windows-x86_64`; `kiri.ps1` у цій теці встановлює
  те саме без MSI; `KIRI_MSI_CULTURE=uk-UA cargo xtask msi` збирає
  українськомовний `Kiri-<version>-x86_64-uk-UA.msi`). Не через TCP:
  localhost-ретранслятор WSL губить напівзакриття з’єднань, і `docker run`
  втрачає свій вивід

Сервіси Kubernetes типу LoadBalancer публікуються на `localhost`, а
`kiri machine create NAME --systemd` запускає systemd як init машини.

## Випуски

Застосунок і DMG підписано Developer ID і нотаризовано. Випуски й канали
оновлень лежать на `https://download.runkiri.com`, і кожен файл та канал
містить у назві архітектуру: `Kiri-<version>-arm64.dmg`,
`Kiri-<version>-arm64.tar.gz` і `latest-mac-arm64.json` для Apple silicon
(`-x86_64` для Mac з Intel), `Kiri-<version>-x86_64.msi` / `-arm64.msi` з
`latest-windows-x86_64.json` / `latest-windows-arm64.json` для Windows.
Кожен застосунок читає лише свій канал, і кожна платформа публікується
окремо. DMG два: `Kiri-<version>-arm64.dmg` містить CLI docker, Compose і
buildx; `Kiri-Lite-<version>-arm64.dmg` (`cargo xtask app-lite`) їх не
містить і завантажує ті самі закріплені версії під час першого запуску
(`crates/kiri/src/cli_pins.rs`, перевірка за SHA-256), якщо на Mac ще
немає CLI docker.

## Відомі проблеми

- **Mac: під’єднаний USB-пристрій зникає з контейнерів, поки Mac спить.**
  macOS знімає USB-пристрої з шини під час сну, тож пристрій на кшталт
  `/dev/ttyACM0` недоступний контейнерам, доки Mac не прокинеться. Потім
  Kiri сам під’єднує його знову. Kiri не тримає Mac увімкненим заради
  пристрою, бо це розряджало б акумулятор ноутбука. Для довгої роботи без
  нагляду (прошивання плат, запис журналу з послідовного порту) не давайте
  Mac заснути, поки вона триває: `caffeinate -s` у терміналі (від мережі)
  або параметр, що забороняє автоматичний сон при вимкненому дисплеї, у
  «Системних параметрах» (на ноутбуці «Акумулятор» › «Параметри», на
  настільному Mac «Енергія»; англійською він зветься «Prevent automatic
  sleeping when the display is off»).

- **`docker build --add-host name:host-gateway` з BuildKit не працює**:
  `host-gateway is not supported by the docker-container driver`. buildx
  відхиляє `host-gateway` для всіх збирачів, крім вбудованого в Docker, а
  збирач Kiri має тип docker-container, тож збірка зупиняється ще до того,
  як її побачить Kiri. Власні docker-container збирачі Docker поводяться
  так само. Доки в Kiri немає вбудованого збирача (заплановано),
  указуйте саму адресу (`--add-host name:172.18.0.1` у Windows і Linux,
  шлюз типового bridge; `198.19.248.254` на Mac) або `DOCKER_BUILDKIT=0`:
  тоді Kiri сам підставляє адресу замість `host-gateway`.
