# Demarrage #

## Ordre de demarrage ##

![schema](img/ordredem.png)

### BIOS - B*asic* I*nput* O*utput* S*ystem* ###

1. Vérification:
* Processeur
* Carte mère

2. Vérification présence périphériques de boot:
* CD rom
* DVD rom
* Clef USB
* Disque dur

3. Active le MBR

### MBR - M*aster* B*oot* R*ecord* - Boot primaire ###

Placé sur le premier secteur adressable d'un disque dur:
* cylindre 0, tête 0 et secteur 1, ou secteur 0 en adressage logique

Taille de 512 octets:
* **446** octets de code exécutable démarré juste après le BIOS
* **64** octets constitué de la table des partition
* **2** octets contiennent le magic number (0x55AA)

![MBR](img/MBR.png)

