# FIFA Soccer 96 (Mega Drive / Genesis) - Archipelago

An Archipelago world for **FIFA Soccer 96** on Sega Mega Drive / Genesis. Play
Friendly matches, unlock team groups and game settings, score cumulative goals,
and send your checks into a multiworld.

The Archipelago game name is **Fifa Soccer 96 Megadrive**. This is a full
Archipelago integration: the game client tracks checks and applies received
items while you play.

## How to play

Each generated seed gives you these starting items:

- **One league or team group**: chosen randomly by default, or selected in the
  YAML options.
- **One weather setting**: randomly chosen from Dry, Damp, Drenched, and Hot.
- **Semi-Pro difficulty**: always your starting difficulty.

Play **Friendly** matches using the team tabs that Archipelago has unlocked.
The game will keep other team groups locked until you receive their unlock
items. Always play on the **Home** side (the team assigned to the left-hand
controller). You may choose any team that Archipelago has unlocked; you are not
restricted to your starting team group.

Checks come from four kinds of achievements:

1. **Score cumulative goals.** The first goal checks `1 goal`, the second checks
   `2 goals`, and so on up to the configured goal count. Goal checks are sent as
   goals are scored; they do not wait for the end of the match.
2. **Beat teams from different groups.** Win a match against a team from each
   of the 17 league or confederation tabs.
3. **Win with a particular setting.** Win with one of the supported weather or
   difficulty settings active.
4. **Win with a supported Special enabled.** These options are in FIFA 96's
   original CHEATS / Secret Menu.

For a win check, finish the match and reach the real **GAME OVER** result. Quitting
a match early does not count as a win. Goal checks earned before quitting remain
separate and are still sent immediately.

Once you have collected the required share of the seed's **Mandatory Medal**
items, Archipelago marks the game complete. The completion goal is separate
from the individual location checks. After returning to the title / Select Mode
menu, the patched game presents its native **CHAMPIONS** screen.

## Checks

| Check group | What you need to do |
| --- | --- |
| Goals | Score goals; each cumulative threshold from `1 goal` through the configured maximum is a location. |
| Beat Teams | Win against a team from each of the 17 groups listed below. |
| Weather | Win with Dry, Damp, Drenched, or Hot weather active. |
| Difficulty | Win in Semi-Pro. The Pro check is optional in YAML. |
| Special | Win with Invisible Walls, Curve Ball, or Crazy Ball active. Balloon Ball is optional in YAML. |

The 17 Beat Team groups are **American, Brazilian, English, French, German,
Italian, Malaysian, Dutch, Scottish, Spanish, Swedish, AFC, CAF, CONCACAF,
CONMEBOL, OFC, and UEFA**. In Archipelago logic, each Beat Team check requires
the matching team-group unlock.

The supported Special checks are **Invisible Walls**, **Curve Ball**,
**Crazy Ball**, and **Balloon Ball**. Only these four original Secret Menu
settings are part of the AP check set. Balloon Ball is optional and is off by
default.

The number of locations is `goal_checks + 25`, plus one if the Pro check is
enabled and one if the Balloon Ball check is enabled. For example, 100 goal
checks with both optional checks enabled produce 127 locations.

## Items

- **Unlock League / Confederation**: 17 items, one for each selectable team
  group. One group is precollected at the start; the rest are shuffled through
  the multiworld.
- **Mandatory Medal**: one item for each configured goal check. These are
  progression items used by the goal logic and the completion goal. The item
  received from a particular check is determined by normal Archipelago item
  placement; a goal check is not guaranteed to award a Medal directly.
- **Unlock Weather**: Dry, Damp, Drenched, and Hot. One is precollected as the
  starting weather; the others are placed in the item pool.
- **Unlock Skill Level**: Semi-Pro is precollected. Pro and its matching check
  are included only when `include_pro_check` is enabled.
- **Unlock Special**: Invisible Walls, Curve Ball, Crazy Ball, and Balloon Ball.
  Balloon Ball and its matching check are included only when
  `include_balloon_ball_check` is enabled.
- **Ball**: filler items used to complete the item pool.

The game client applies received unlocks and displays incoming item messages in
BizHawk. The client also keeps supported settings tied to the items actually
received, so a setting that is still locked cannot be used to satisfy its win
check.

## Cumulative goals and Mandatory Medals

`goal_checks` sets both the number of cumulative goal locations and the number
of **Mandatory Medal** items in the world. For example, with 50 goal checks,
the locations are `1 goal` through `50 goals`, and the world contains 50
Mandatory Medals.

Goal locations are gated in Archipelago logic in blocks of ten:

- Goals 1-10 require no Medals.
- Goals 11-20 require at least 5 Medals.
- Goals 21-30 require at least 10 Medals.
- Each further block of ten requires five additional Medals.

The in-game goal checks are still cumulative and immediate. The block rules
control the logic and item placement for the multiworld; they are not a limit on
how many goals you may score in a match.

The victory threshold is calculated from the configured goal count and
percentage, rounded up. For example, 50 goals at 80 percent requires 40
Mandatory Medals. At 100 percent, all configured Medals are required. The
minimum victory requirement is one Medal.

## YAML options

Create a player YAML with Archipelago's **Options Creator**, select **Fifa
Soccer 96 Megadrive**, and edit the game-specific options below. The keys are
written in English and should stay exactly as shown.

| YAML option | Default | Allowed values | Effect |
| --- | ---: | --- | --- |
| `starting_league` | `shuffle` | `shuffle`, or one of the 17 group names | `shuffle` chooses one playable group when the seed is generated. A named value forces that starting group. |
| `goal_checks` | `50` | `10`-`100` | Number of cumulative goal checks and Mandatory Medal items. |
| `goal_medals_percent` | `80` | `10`-`100` | Percentage of the Medal items required for Archipelago completion. Rounded up. |
| `include_pro_check` | `false` | `true` / `false` | Adds both the Pro-difficulty win check and its unlock item. Off by default. |
| `include_balloon_ball_check` | `false` | `true` / `false` | Adds both the Balloon Ball win check and its unlock item. Off by default. |

Valid fixed values for `starting_league` are `american`, `brazilian`, `english`,
`french`, `german`, `italian`, `malaysian`, `dutch`, `scottish`, `spanish`,
`swedish`, `afc`, `caf`, `concacaf`, `conmebol`, `ofc`, and `uefa`.

There are no YAML options to choose a starting weather or starting difficulty:
the seed randomizes one starting weather, while Semi-Pro is always precollected.

### Example YAML

```yaml
name: Player1
game: Fifa Soccer 96 Megadrive
description: Archipelago for Fifa Soccer 96 on Sega Megadrive Genesis

"Fifa Soccer 96 Megadrive":
  starting_league: shuffle
  goal_checks: 50
  goal_medals_percent: 80
  include_pro_check: false
  include_balloon_ball_check: false
```

The `name` must match your slot name when connecting to the room. Archipelago
may add its normal multiworld options to the YAML; those are not FIFA 96-specific.

## Changes from the original game

- The original broad **World** team group is divided into **AFC, CAF, CONCACAF,
  CONMEBOL, OFC, and UEFA**, so these confederations can be unlocked separately.
- The patched title/menu flow focuses on the supported Archipelago **Friendly**
  route. Unused original menu slots are blanked or removed, and team tabs stay
  locked until their corresponding unlock item is received.
- Weather, difficulty, and supported Special settings are gated by Archipelago
  items. Weather and difficulty are set through the game's regular settings;
  the Special effects are selected in the original CHEATS / Secret Menu. The
  patch exposes that menu through **OPTIONS -> Special**.
- The Archipelago experience and patched in-game text are **English-only**.
  Although I am French, supporting every language would require separate menu
  text, layout, and patch work for each version of the game, plus extensive
  testing. English is the supported language for this project. This README is
  also available in French below.
- Archipelago completion is added as the victory condition. Reaching the
  configured Medal threshold completes the AP goal and leads to the native
  CHAMPIONS presentation; it is not an extra fillable location.

## Installation and setup

You need [Archipelago](https://archipelago.gg), BizHawk with the **Genesis Plus
GX** core, the FIFA 96 APWorld, and an unmodified copy of the base ROM named
`FIFA Soccer 96 (USA, Europe) (En,Fr,De,Es,It,Sv).md`.
Its SHA-256 is `6a4d606c6f1fa8930e00d317dc8d3edee8f54ac2df2ac835bcb6be6777c35b01`.
The game ROM is not distributed by this project; use your own copy matching
this reference.

1. Download the latest `fifa96.apworld` from this repository's GitHub Releases.
2. Install it using Archipelago Launcher's **Install APWorld** option, or place
   it in the Archipelago `custom_worlds` folder. Restart the Launcher so it
   loads the world.
3. Set `fifa96_options.rom_file` in Archipelago's `host.yaml` to the path to
   your own unmodified ROM matching the reference filename and SHA-256 above.
   Neither the ROM nor a pre-patched game is distributed here.
4. Create a YAML for **Fifa Soccer 96 Megadrive** with the Options Creator, or
   use the example above. Place the YAML in Archipelago's `Players` folder and
   generate the multiworld.
5. Archipelago creates a `.apfifa96` patch for each FIFA 96 slot. Give each
   player the patch for their own slot. Open it with Archipelago Launcher
   (**Open Patch**) to apply it to that player's configured base ROM. The patch
   produces a `.md` game image for that seed.
6. Open the resulting `.md` image in BizHawk using Genesis Plus GX. Start the
   Archipelago BizHawk Client, connect to the room address, and enter the exact
   slot name from the YAML `name` field.
7. Wait for the client to connect and recognize the game, then play Friendly
   matches. Keep the client connected so it can send checks and apply received
   unlocks.

Do not load the unpatched vanilla ROM for an Archipelago seed: the seed-specific
`.apfifa96` patch must be applied first.

## Documentation

The `docs/` directory contains three standalone references:

- `FIFA96_MEGADRIVE_MEMORY_MAP.txt` - the game's memory-map reference.
- `MEGADRIVE_ARCHIPELAGO_ROMHACKING_GUIDE.txt` - a general Mega Drive
  Archipelago ROM-hacking guide.
- `ARCHIPELAGO_INTEGRATION_PROJECT_HISTORY.txt` - the dated development
  history, decisions, beta progress, and remaining limitations.

## Acknowledgements

Thank you to **UltiNaruto** for the [Streets of Rage Archipelago APWorld and
source](https://github.com/UltiNaruto/SOR_AP_Randomizer) ([releases](https://github.com/UltiNaruto/SOR_AP_Randomizer/releases/latest)), and to **kaithar** for
the [Sonic the Hedgehog 1 Mega Drive APWorld source](https://github.com/kaithar/Archipelago/tree/sonic1/worlds/sonic1) ([release](https://github.com/kaithar/Archipelago/releases/tag/sonic_1-v0.1.1)).
Reading these two APWorlds was extremely important to creating this project.

## AI disclosure

This project was developed entirely with AI, specifically **Arena.ai Agent
Mode**. All programming, ROM hacking, Archipelago integration, technical
analysis, and documentation - including this README - were AI-produced.
Satanos supplied the goals, design decisions, and feedback, and carried out the
gameplay test runs.

---

# Version française

Un monde Archipelago pour **FIFA Soccer 96** sur Sega Mega Drive / Genesis.
Jouez des matchs amicaux, débloquez les groupes d'équipes et les réglages,
marquez des buts cumulatifs et envoyez vos checks dans un multiworld.

Le nom du jeu dans Archipelago est **Fifa Soccer 96 Megadrive**. Il s'agit d'une
intégration complète : le client suit les checks et applique les objets reçus
pendant que vous jouez.

## Comment jouer

Chaque seed générée vous donne ces objets de départ :

- **Une ligue ou un groupe d'équipes** : choisi au hasard par défaut, ou fixé
  dans les options du YAML.
- **Un réglage de météo** : choisi au hasard parmi Dry, Damp, Drenched et Hot.
- **La difficulté Semi-Pro** : c'est toujours votre difficulté de départ.

Jouez en mode **Friendly** avec les onglets d'équipes débloqués par Archipelago.
Les autres groupes restent verrouillés jusqu'à la réception de leurs objets de
déblocage. Jouez toujours du côté **Home** (la manette située à gauche). Vous
pouvez choisir n'importe quelle équipe débloquée par Archipelago; vous n'êtes
pas limité à votre groupe d'équipes de départ.

Les checks viennent de quatre types de progression :

1. **Marquer des buts cumulatifs.** Le premier but valide `1 goal`, le deuxième
   `2 goals`, et ainsi de suite jusqu'au nombre configuré. Les checks de buts
   sont envoyés au fil des buts; ils n'attendent pas la fin du match.
2. **Battre des équipes de groupes différents.** Remporter un match contre une
   équipe de chacun des 17 onglets de ligue ou de confédération.
3. **Gagner avec un réglage donné.** Gagner avec l'un des réglages de météo ou
   de difficulté pris en charge.
4. **Gagner avec un Special activé.** Ces options se trouvent dans le menu
   CHEATS / Secret Menu d'origine de FIFA 96.

Pour valider un check de victoire, terminez le match et atteignez le véritable
écran **GAME OVER**. Quitter un match avant la fin ne compte pas comme une
victoire. Les checks de buts obtenus avant de quitter restent distincts et sont
envoyés immédiatement.

Une fois la proportion requise d'objets **Mandatory Medal** obtenue, Archipelago
marque le jeu comme terminé. Cette condition de victoire est séparée des
locations individuelles. Après le retour au menu titre / Select Mode, le jeu
affiche son écran natif **CHAMPIONS**.

## Checks

| Groupe de checks | Ce qu'il faut faire |
| --- | --- |
| Buts | Marquer des buts; chaque seuil cumulatif de `1 goal` au maximum configuré constitue une location. |
| Beat Teams | Gagner contre une équipe de chacun des 17 groupes ci-dessous. |
| Météo | Gagner avec la météo Dry, Damp, Drenched ou Hot active. |
| Difficulté | Gagner en Semi-Pro. Le check Pro est optionnel dans le YAML. |
| Special | Gagner avec Invisible Walls, Curve Ball ou Crazy Ball actif. Balloon Ball est optionnel dans le YAML. |

Les 17 groupes Beat Team sont **American, Brazilian, English, French, German,
Italian, Malaysian, Dutch, Scottish, Spanish, Swedish, AFC, CAF, CONCACAF,
CONMEBOL, OFC et UEFA**. Dans la logique Archipelago, chaque check Beat Team
nécessite l'objet de déblocage du groupe correspondant.

Les Special pris en charge sont **Invisible Walls**, **Curve Ball**,
**Crazy Ball** et **Balloon Ball**. Seuls ces quatre réglages du Secret Menu
d'origine font partie des checks AP. Balloon Ball est optionnel et désactivé
par défaut.

Le nombre de locations est `goal_checks + 25`, auquel on ajoute une location si
le check Pro est activé et une si le check Balloon Ball est activé. Par exemple,
100 checks de buts avec les deux options activées donnent 127 locations.

## Objets

- **Unlock League / Confederation** : 17 objets, un pour chaque groupe
  d'équipes. Un groupe est précollecté au départ; les autres sont répartis dans
  le multiworld.
- **Mandatory Medal** : un objet pour chaque check de but configuré. Ce sont
  des objets de progression utilisés par la logique des buts et la condition de
  victoire. L'objet reçu après un check dépend du placement normal d'Archipelago;
  un check de but ne donne pas forcément directement une Medal.
- **Unlock Weather** : Dry, Damp, Drenched et Hot. Une météo est précollectée
  au départ; les autres sont placées dans le pool d'objets.
- **Unlock Skill Level** : Semi-Pro est précollecté. Pro et son check associé
  ne sont inclus que si `include_pro_check` est activé.
- **Unlock Special** : Invisible Walls, Curve Ball, Crazy Ball et Balloon Ball.
  Balloon Ball et son check associé ne sont inclus que si
  `include_balloon_ball_check` est activé.
- **Ball** : objets de remplissage qui complètent le pool.

Le client du jeu applique les objets reçus et affiche les notifications dans
BizHawk. Il lie aussi les réglages pris en charge aux objets effectivement
reçus : un réglage encore verrouillé ne peut pas valider son check de victoire.

## Buts cumulatifs et Mandatory Medals

`goal_checks` définit à la fois le nombre de locations de buts cumulatifs et le
nombre d'objets **Mandatory Medal** dans le monde. Par exemple, avec 50 checks
de buts, les locations vont de `1 goal` à `50 goals` et le monde contient 50
Mandatory Medals.

Dans la logique Archipelago, les locations de buts sont verrouillées par blocs
de dix :

- Les buts 1 à 10 ne nécessitent aucune Medal.
- Les buts 11 à 20 nécessitent au moins 5 Medals.
- Les buts 21 à 30 nécessitent au moins 10 Medals.
- Chaque bloc de dix suivant nécessite cinq Medals supplémentaires.

Les checks du jeu restent cumulatifs et immédiats. Les règles par blocs
contrôlent la logique et le placement des objets dans le multiworld; elles ne
limitent pas le nombre de buts que vous pouvez marquer dans un match.

Le seuil de victoire est calculé à partir du nombre de buts configuré et du
pourcentage, avec un arrondi à l'entier supérieur. Par exemple, 50 buts à 80 %
nécessitent 40 Mandatory Medals. À 100 %, toutes les Medals configurées sont
nécessaires. Le minimum requis pour gagner est une Medal.

## Options YAML

Créez un YAML joueur avec l'**Options Creator** d'Archipelago, sélectionnez
**Fifa Soccer 96 Megadrive**, puis modifiez les options spécifiques au jeu
ci-dessous. Les clés s'écrivent en anglais et doivent rester exactement comme
indiqué.

| Option YAML | Défaut | Valeurs possibles | Effet |
| --- | ---: | --- | --- |
| `starting_league` | `shuffle` | `shuffle` ou l'un des 17 noms de groupe | `shuffle` choisit un groupe jouable lors de la génération. Une valeur nommée impose le groupe de départ. |
| `goal_checks` | `50` | `10`-`100` | Nombre de checks de buts cumulatifs et d'objets Mandatory Medal. |
| `goal_medals_percent` | `80` | `10`-`100` | Pourcentage de Medals nécessaire à la complétion Archipelago. Arrondi au supérieur. |
| `include_pro_check` | `false` | `true` / `false` | Ajoute le check de victoire en Pro et son objet de déblocage. Désactivé par défaut. |
| `include_balloon_ball_check` | `false` | `true` / `false` | Ajoute le check de victoire avec Balloon Ball et son objet de déblocage. Désactivé par défaut. |

Les valeurs fixes possibles pour `starting_league` sont `american`, `brazilian`,
`english`, `french`, `german`, `italian`, `malaysian`, `dutch`, `scottish`,
`spanish`, `swedish`, `afc`, `caf`, `concacaf`, `conmebol`, `ofc` et `uefa`.

Il n'existe pas d'option YAML pour choisir la météo ou la difficulté de départ :
la seed choisit une météo au hasard, tandis que Semi-Pro est toujours
précollecté.

### Exemple de YAML

```yaml
name: Player1
game: Fifa Soccer 96 Megadrive
description: Archipelago for Fifa Soccer 96 on Sega Megadrive Genesis

"Fifa Soccer 96 Megadrive":
  starting_league: shuffle
  goal_checks: 50
  goal_medals_percent: 80
  include_pro_check: false
  include_balloon_ball_check: false
```

Le champ `name` doit correspondre au nom de slot utilisé pour rejoindre la
room. Archipelago peut ajouter ses options habituelles de multiworld; elles ne
sont pas propres à FIFA 96.

## Changements par rapport au jeu d'origine

- Le grand groupe d'équipes **World** d'origine est divisé en **AFC, CAF,
  CONCACAF, CONMEBOL, OFC et UEFA**, afin que chaque confédération puisse être
  débloquée séparément.
- Le parcours et les menus modifiés sont centrés sur le mode **Friendly** pris
  en charge par Archipelago. Les emplacements de menu d'origine inutilisés sont
  vidés ou supprimés, et les onglets d'équipes restent verrouillés jusqu'à la
  réception de l'objet de déblocage correspondant.
- La météo, la difficulté et les Special pris en charge sont liés aux objets
  Archipelago. La météo et la difficulté se règlent dans les menus ordinaires du
  jeu; les effets Special se choisissent dans le menu CHEATS / Secret Menu
  d'origine. Le patch rend ce menu accessible depuis **OPTIONS -> Special**.
- L'expérience Archipelago et les textes du jeu patché sont disponibles
  **uniquement en anglais**. Bien que je sois français, localiser chaque menu,
  texte et mise en page pour toutes les versions linguistiques du jeu
  nécessiterait un travail et des tests distincts pour chaque langue. L'anglais
  est la seule langue prise en charge par ce projet. Le README est également
  disponible en français ci-dessus.
- La complétion Archipelago constitue la condition de victoire. Lorsque le
  seuil de Medals configuré est atteint, l'objectif AP est terminé et le jeu
  présente l'écran natif CHAMPIONS; il ne s'agit pas d'une location
  supplémentaire à remplir.

## Installation et configuration

Il vous faut [Archipelago](https://archipelago.gg), BizHawk avec le cœur
**Genesis Plus GX**, l'APWorld FIFA 96 et une copie non patchée de la ROM de base
`FIFA Soccer 96 (USA, Europe) (En,Fr,De,Es,It,Sv).md`.
Son SHA-256 est `6a4d606c6f1fa8930e00d317dc8d3edee8f54ac2df2ac835bcb6be6777c35b01`.
La ROM du jeu n'est pas distribuée par ce projet; utilisez votre propre copie
correspondant à cette référence.

1. Téléchargez le dernier fichier `fifa96.apworld` depuis les Releases GitHub
   de ce dépôt.
2. Installez-le avec l'option **Install APWorld** d'Archipelago Launcher, ou
   placez-le dans le dossier `custom_worlds` d'Archipelago. Redémarrez le
   Launcher pour qu'il charge le monde.
3. Dans le `host.yaml` d'Archipelago, indiquez sous
   `fifa96_options.rom_file` le chemin de votre propre ROM non patchée. Elle
   doit correspondre au nom de fichier et au SHA-256 indiqués ci-dessus. Ni la
   ROM ni un jeu déjà patché ne sont distribués ici.
4. Créez un YAML pour **Fifa Soccer 96 Megadrive** avec Options Creator, ou
   utilisez l'exemple ci-dessus. Placez le YAML dans le dossier `Players`
   d'Archipelago et générez le multiworld.
5. Archipelago crée un patch `.apfifa96` pour chaque slot FIFA 96. Donnez à
   chaque joueur le patch correspondant à son slot. Ouvrez-le avec Archipelago
   Launcher (**Open Patch**) pour l'appliquer à la ROM de base configurée sur son
   ordinateur. Le patch produit une image `.md` propre à cette seed.
6. Ouvrez l'image `.md` obtenue dans BizHawk avec Genesis Plus GX. Lancez le
   BizHawk Client d'Archipelago, connectez-vous à l'adresse de la room et
   saisissez exactement le nom de slot indiqué dans le champ YAML `name`.
7. Attendez que le client se connecte et reconnaisse le jeu, puis jouez en mode
   Friendly. Gardez le client connecté pour qu'il envoie les checks et applique
   les objets reçus.

Ne chargez pas la ROM vanilla non patchée pour une seed Archipelago : il faut
d'abord appliquer le patch `.apfifa96` propre à la seed.

## Documentation

Le dossier `docs/` contient trois références autonomes :

- `FIFA96_MEGADRIVE_MEMORY_MAP.txt` - la carte mémoire du jeu.
- `MEGADRIVE_ARCHIPELAGO_ROMHACKING_GUIDE.txt` - un guide général de ROM
  hacking Archipelago sur Mega Drive.
- `ARCHIPELAGO_INTEGRATION_PROJECT_HISTORY.txt` - l'historique daté du
  développement, les décisions, les résultats du bêta et les limites restantes.

## Remerciements

Merci à **UltiNaruto** pour l'[APWorld Archipelago de Streets of Rage et son
code source](https://github.com/UltiNaruto/SOR_AP_Randomizer) ([dernières
releases](https://github.com/UltiNaruto/SOR_AP_Randomizer/releases/latest)), et à
**kaithar** pour le [code source de l'APWorld Mega Drive de Sonic the Hedgehog 1](https://github.com/kaithar/Archipelago/tree/sonic1/worlds/sonic1) ([release](https://github.com/kaithar/Archipelago/releases/tag/sonic_1-v0.1.1)).
La lecture de ces deux APWorlds a été très importante pour créer ce projet.

## Transparence sur l'utilisation de l'IA

Ce projet a été développé entièrement avec l'IA, plus précisément avec
**Arena.ai Agent Mode**. La programmation, le ROM hacking, l'intégration
Archipelago, l'analyse technique et la documentation - y compris ce README -
ont été produits par IA. Satanos a fourni les objectifs, les choix de conception
et les retours, et a effectué les sessions de test du jeu.
