# damageengine-api — dépôt Maven

Ce dépôt **ne contient pas de code source**. C'est un dépôt Maven servi par
`raw.githubusercontent`, qui publie l'API de
[DamageEngine](https://github.com/Beltaria/damage-engine) pour que les autres
plugins compilent contre elle sans avoir à construire le moteur localement.

Les artefacts sont publiés automatiquement par le workflow de release de
DamageEngine, à chaque tag `v*`. **Rien ici ne s'édite à la main.**

## Utiliser l'API

L'artefact `com.beltaria:damage-engine-api` ne contient que le paquet
`com.beltaria.damageengine.api` — `DamageComputeEvent`, `DamageFormula`,
`ContextKey`. Les classes internes du moteur n'y sont pas : l'intérieur peut
être retravaillé sans casser les plugins qui s'y branchent.

**Gradle** (Kotlin DSL)

```kotlin
repositories {
    maven("https://raw.githubusercontent.com/Beltaria/damageengine-api/main/") {
        content { includeGroup("com.beltaria") }
    }
}

dependencies {
    compileOnly("com.beltaria:damage-engine-api:1.0.0")
}
```

**Maven**

```xml
<repositories>
  <repository>
    <id>damageengine</id>
    <url>https://raw.githubusercontent.com/Beltaria/damageengine-api/main/</url>
  </repository>
</repositories>

<dependencies>
  <dependency>
    <groupId>com.beltaria</groupId>
    <artifactId>damage-engine-api</artifactId>
    <version>1.0.0</version>
    <scope>provided</scope>
  </dependency>
</dependencies>
```

`compileOnly` / `provided` est obligatoire, pas une optimisation de taille :
l'API est fournie à l'exécution par le plugin DamageEngine installé sur le
serveur. Il faut aussi le déclarer dans votre `plugin.yml` :

```yaml
depend: [DamageEngine]      # ou softdepend, pour une intégration optionnelle
```

Gardez par ailleurs votre dépendance d'API serveur déclarée : l'artefact n'en
déclare aucune, et les signatures publiques exposent des types Bukkit.

La documentation complète — contrat de nommage, formule, phases de listener,
pièges — est dans le [README de
DamageEngine](https://github.com/Beltaria/damage-engine).

## Versions publiées

Une version publiée est **immuable** : le workflow ajoute un dossier de version
et refuse d'écraser une version existante. Pour corriger une release, il faut en
publier une nouvelle.

Les artefacts vivent sous `com/beltaria/damage-engine-api/<version>/` : le jar,
son POM, les sources et la javadoc.
