# Небезопасная уязвимость десериализации во многих модах Minecraft

Несколько недель назад во многих модах Minecraft была обнаружена очень критическая уязвимость, позволяющая выполнять произвольный удаленный код на клиентах и серверах (и, следовательно, даже на всех подключенных клиентах на сервере).

Первоначально мы пытались исследовать всю проблему в частном порядке и ответственно, чтобы мы могли опубликовать обширную статью и исправить всю ситуацию, но поскольку группа
под названием MMPA только что опубликовал [сообщение в блоге](https://blog.mmpa.info/posts/bleeding-pipe/) о проблеме, полностью упустив многие важные факторы, связанные с проблемой, мы были вынуждены выпустить заявление и попытаться решить проблему немедленно, так как в
в настоящее время они буквально подвергают риску миллионы модифицированных пользователей Minecraft.

## Информация об уязвимости

Уязвимость вызвана небезопасным использованием функции сериализации Java в сетевых пакетах, отправляемых серверами клиентам или клиентами серверам, что позволяет создавать экземпляры любого класса Java, загруженного в экземпляр Minecraft.

В прошлом уже существовала подобная уязвимость под названием «Mad Gadget». Подробнее об этом можно прочитать здесь:
- https://opensource.googleblog.com/2017/03/operation-rosehub.html
- https://foxglovesecurity.com/2015/11/06/what-do-weblogic-websphere-jboss-jenkins-opennms-and-your-application-have-in-common-this-vulnerability/

Несмотря на то, что существует относительно небольшое количество атак, нацеленных на эту уязвимость, из-за важности уязвимости играть с неисправленными модами в настоящее время совершенно опасно.
Злоумышленники уже пытались (и в некоторых случаях преуспели) похитить токен доступа Microsoft и сеанс браузера. Но поскольку они могут буквально выполнять любой код, какой захотят, в целевой системе, возможности безграничны.

## Как защититься от уязвимости?

Мы разработали патчер, который пытается исправить все известные на данный момент уязвимые моды (перечислены ниже).

~~ Если будут обнаружены какие-либо другие затронутые моды, исправление так же просто, как обновление соответствующего файла конфигурации. (Мы опубликуем релиз, который автоматизирует это для вас) ~~ Версия 1.3 патча теперь автоматически использует последнюю версию [файла конфигурации] (https://github.com/dogboy21/serializationisbad/blob/master/serializationisbad .json) и в противном случае возвращается к локальному файлу конфигурации. Если конфигурация отсутствует, должна появиться ошибка, информирующая пользователя о том, что в настоящее время не применяются исправления.

### Minecraft Forge 1.7.x — последняя версия

- Загрузите файл JAR из последней версии на [странице релизов] (https://github.com/dogboy21/serializationisbad/releases)
   - Исправление теперь также доступно на [CurseForge] (https://www.curseforge.com/minecraft/mc-mods/serializationisbad) (выпуск Modrinth в настоящее время находится на рассмотрении)
- Добавьте файл JAR в папку с модами.
- ~~ Загрузите последний файл конфигурации из [этого репозитория Github] (https://github.com/dogboy21/serializationisbad/blob/master/serializationisbad.json) и добавьте его непосредственно в каталог конфигурации ваших экземпляров~~ Версия 1.3 патч теперь автоматически использует последнюю версию [файла конфигурации] (https://github.com/dogboy21/serializationisbad/blob/master/serializationisbad.json)

### Любые другие экземпляры

- Загрузите файл JAR из последней версии на [странице релизов] (https://github.com/dogboy21/serializationisbad/releases) и сохраните его где-нибудь.
- Добавьте следующий аргумент JVM к вашему клиенту/серверу (о том, как это сделать, обратитесь к документации программы запуска клиента/сервера, которую вы используете): `-javaagent:<ПУТЬ К СОХРАНЕННОМУ ФАЙЛУ JAR>`
- ~~ Загрузите последний файл конфигурации из [этого репозитория Github] (https://github.com/dogboy21/serializationisbad/blob/master/serializationisbad.json) и добавьте его непосредственно в каталог конфигурации ваших экземпляров~~ Версия 1.3 патч теперь автоматически использует последнюю версию [файла конфигурации] (https://github.com/dogboy21/serializationisbad/blob/master/serializationisbad.json)

## Затронутые моды

В отличие от приведенного выше сообщения в блоге, эта проблема затрагивает гораздо больше модов.
Хотя некоторые из них уже исправлены в последних версиях, эти моды можно было использовать по крайней мере в одной более старой версии:

**ПОМНИТЕ, ЧТО ЭТОТ СПИСОК ОПРЕДЕЛЕННО НЕ ПОЛНЫЙ. ЭТО ТОЛЬКО МОДЫ, КОТОРЫЕ МЫ В НАСТОЯЩЕЕ ВРЕМЯ ЗНАЕМ. По крайней мере, Curseforge уже занимается внутренним расследованием проблемы, так что в будущем мы, возможно, сможем получить почти полный список уязвимых модов и версий.**

Из-за поспешного объявления мы в настоящее время не можем указать точные диапазоны версий затронутых модов. Если вы хотите помочь с этим, не стесняйтесь вносить свой вклад в этот список.

- [AetherCraft](https://www.curseforge.com/minecraft/mc-mods/aec)
- [Advent of Ascension (Nevermine)](https://www.curseforge.com/minecraft/mc-mods/advent-of-ascension-nevermine) (Only affects versions for Minecraft 1.12.2)
- [Arrows Plus](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-mods/1290719-1-6-2-ssp-smp-arrows-plus-v1-0-0-minecraft)
- [Astral Sorcery](https://www.curseforge.com/minecraft/mc-mods/astral-sorcery) (affected versions: <=1.9.1)
- [BdLib](https://www.curseforge.com/minecraft/mc-mods/bdlib) (Only affects versions for Minecraft 1.7.10-1.12.2)
- [Carbonization](https://www.curseforge.com/minecraft/mc-mods/carbonization)
- [CreativeCore](https://www.curseforge.com/minecraft/mc-mods/creativecore) (Only affects versions for Minecraft 1.7.10)
- [Custom Friends Capes](https://www.curseforge.com/minecraft/mc-mods/custom-friends-capes)
- [CustomOreGen](https://www.curseforge.com/minecraft/mc-mods/customoregen)
- [DankNull](https://www.curseforge.com/minecraft/mc-mods/dank-null)
- [Energy Manipulation](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-mods/1290125-1-6-4-1-6-2-1-5-2-1-4-7-energy-manipulation-1-1)
- [EnderCore](https://www.curseforge.com/minecraft/mc-mods/endercore) (Only affects versions for Minecraft 1.9-1.13)
- [EndermanEvolution](https://www.curseforge.com/minecraft/mc-mods/enderman-evolution)
- Extrafirma
- [Gadomancy](https://www.curseforge.com/minecraft/mc-mods/gadomancy)
- [Giacomo's Bookshelf](https://www.curseforge.com/minecraft/mc-mods/giacomos-bookshelf)
- [Immersive Armors](https://www.curseforge.com/minecraft/mc-mods/immersive-armors) (Fixed in version 1.5.6 for Minecraft 1.18.2, 1.19.2-1.19.4, 1.20, versions for 1.16.5, 1.17.1, 1.18.1, 1.19.0, 1.19.1 remain affected, [relevant commit](https://github.com/Luke100000/ImmersiveArmors/issues/68))
- [Immersive Aircraft](https://www.curseforge.com/minecraft/mc-mods/immersive-aircraft)
- [Immersive Paintings](https://www.curseforge.com/minecraft/mc-mods/immersive-paintings)
- [JourneyMap](https://www.curseforge.com/minecraft/mc-mods/journeymap) (Fixed introduced in 1.16.5-5.7.1 and fixed in 1.16.5-5.7.2 No other versions were effected)
- [LanteaCraft / SGCraft](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-mods/1292427-lanteacraft)
- [LogisticsPipes](https://www.curseforge.com/minecraft/mc-mods/logistics-pipes) (Only affects versions for Minecraft 1.4.7-1.7.10. Fixed in version 0.10.0.71 for MC 1.7.10, [relevant security advisory](https://github.com/RS485/LogisticsPipes/security/advisories/GHSA-mcp7-xf3v-25x3))
- [Minecraft Comes Alive (MCA)](https://www.curseforge.com/minecraft/mc-mods/minecraft-comes-alive-mca) (Only affects versions for Minecraft 1.5.2-1.6.4)
- [MattDahEpic Core (MDECore)](https://www.curseforge.com/minecraft/mc-mods/mattdahepic-core) (Only affects versions for Minecraft 1.8.8-1.12.2)
- [mxTune](https://www.curseforge.com/minecraft/mc-mods/mxtune) (Only affects versions for Minecraft 1.12-1.16.5)
- [p455w0rd's Things](https://www.curseforge.com/minecraft/mc-mods/p455w0rds-things)
- [Project Blue](https://www.csse.canterbury.ac.nz/greg.ewing/minecraft/mods/ProjectBlue/)
- [RadixCore](https://www.curseforge.com/minecraft/mc-mods/radixcore)
- [RebornCore](https://www.curseforge.com/minecraft/mc-mods/reborncore) (affected versions: >= 3.13.8, <4.7.3, [relevant security advisory](https://github.com/TechReborn/RebornCore/security/advisories/GHSA-r7pg-4xrf-7mrm))
- [SimpleAchievements](https://www.curseforge.com/minecraft/mc-mods/simple-achievements)
- [SmartMoving](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-mods/1274224-smart-moving)
- [Strange](https://www.curseforge.com/minecraft/mc-mods/strange)
- [SuperMartijn642's Config Lib](https://www.curseforge.com/minecraft/mc-mods/supermartijn642s-config-lib) (Fixed in version 1.0.9, [relevant security advisory](https://github.com/SuperMartijn642/SuperMartijn642sConfigLib/security/advisories/GHSA-f4r5-w453-2jx6))
- [Thaumic Tinkerer](https://www.curseforge.com/minecraft/mc-mods/thaumic-tinkerer) (Fixed in version 2.3-138 for Minecraft 1.7.2, versions for 1.6-1.6.4 remain affected, [relevant commit](https://github.com/Thaumic-Tinkerer/ThaumicTinkerer/commit))
- [Tough Expansion](https://www.curseforge.com/minecraft/mc-mods/tough-expansion)
- [ttCore](https://www.curseforge.com/minecraft/mc-mods/ttcore) (Only affects versions for Minecraft 1.7.10)

## Кредиты

Я не единственный, кто работал над расследованием всей ситуации.

Credits to anyone that was involved in this:

- Aidoneus (MineYourMind Server Network)
- bziemons (Logistics Pipes Mod Developer)
- Bennyboy1695 (Shadow Node Server Network)
- Dogboy21 (MyFTB Server Network)
- Einhornyordle (MyFTB Server Network)
- emily (CraftDownUnder Server Network)
- Exa (Nomifactory Modpack Developer)
- HanoverFist (MineYourMind Server Network)
- HellFirePvP (Astral Sorcery Mod Developer)
- Jacob (DirtCraft Server Network)
- Juakco_ (CraftDownUnder Server Network)
- Lìam (MineYourMind Server Network)
- MojangPlsFix (MyFTB Server Network)
- Heather (MMCC Server Network)
- Niels Pilgaard (Enigmatica Modpack Developer)
- oliviajumba (CraftDownUnder Server Network)
- oly2o6 (All the Mods Modpack Developer / Akliz Server Hoster)
- PurpleIsEverything (Shadow Node Server Network)
- Pyker (Technic Launcher Developer)
- RyanTheAllmighty (ATLauncher Developer)
- Saereth (Modpack Developer)
- Sauramel (CraftDownUnder Server Network)
- ThePixelbrain (MMCC Server Network)
- Tridos (DirtCraft Server Network)
- DarkStar (CraftDownUnder Server Network)
