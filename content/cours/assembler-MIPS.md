+++
title = "Cheatsheet MIPS"
date = 2026-05-05
categories = ["Cheatsheet"]
description = "Cheatsheet des registres, instructions et syntaxe MIPS"


[taxonomies]
tags = ["cheatsheet", "firmware", "asm", "MIPS"]
+++

# Registres:

| N° de registre | Nom   | Fonction du registre                                         |
| -------------- | ----- | ------------------------------------------------------------ |
| 0              | $zero | Toujours égal à 0                                            |
| 1              | $at   | Réservé à l'assembleur                                       |
| 2              | $v0   | Stocker le resultat des fonctions                            |
| 3              | $v1   | Stocker le résultat des fonctions                            |
| 4              | $a0   | Permet de stocker les 4 premiers arguments du sous programme |
| 5              | $a1   | Permet de stocker les 4 premiers arguments du sous programme |
| 6              | $a2   | Permet de stocker les 4 premiers arguments du sous programme |
| 7              | $a3   | Permet de stocker les 4 premiers arguments du sous programme |
| 8              | $t0   | Registre temporaire                                          |
| 9              | $t1   | Registre temporaire                                          |
| 10             | $t2   | Registre temporaire                                          |
| 11             | $t3   | Registre temporaire                                          |
| 12             | $t4   | Registre temporaire                                          |
| 13             | $t5   | Registre temporaire                                          |
| 14             | $t6   | Registre temporaire                                          |
| 15             | $t7   | Registre temporaire                                          |
| 16             | $s0   | Registres sauvegardés et utilisés plus tard                  |
| 17             | $s1   | Registres sauvegardés et utilisés plus tard                  |
| 18             | $s2   | Registres sauvegardés et utilisés plus tard                  |
| 19             | $s3   | Registres sauvegardés et utilisés plus tard                  |
| 20             | $s4   | Registres sauvegardés et utilisés plus tard                  |
| 21             | $s5   | Registres sauvegardés et utilisés plus tard                  |
| 22             | $s6   | Registres sauvegardés et utilisés plus tard                  |
| 23             | $s7   | Registres sauvegardés et utilisés plus tard                  |
| 24             | $t8   | Registre temporaire                                          |
| 25             | $t9   | Registre temporaire                                          |
| 26             | $k0   | Registre réservé au système                                  |
| 27             | $k1   | Registre réservé au système                                  |
| 28             | $gp   | Registre du pointeur général                                 |
| 29             | $sp   | Registre du pointeur de la stack                             |
| 30             | $fp   | Registre du frame pointer                                    |
| 31             | $ra   | Contient l'adresse de retour                                 |
# Instructions
| Instruction | Fonction (explication simple)                        | Type | Arguments                          |
| ----------- | ---------------------------------------------------- | ---- | ---------------------------------- |
| **add**     | Additionne deux registres signés                     | R    | `add $d, $s, $t` → $d = $s + $t    |
| **addi**    | Additionne une constante (immédiate) à un registre   | I    | `addi $t, $s, val` → $t = $s + val |
| **addiu**   | Additionne immédiate sans signe (ignore overflow)    | I    | `addiu $t, $s, val`                |
| **addu**    | Additionne sans signe (ignore overflow)              | R    | `addu $d, $s, $t`                  |
| **and**     | Fait un ET logique entre 2 registres                 | R    | `and $d, $s, $t`                   |
| **andi**    | Fait un ET logique avec une valeur immédiate         | I    | `andi $t, $s, val`                 |
| **beq**     | Saute à une adresse si 2 registres sont égaux        | I    | `beq $s, $t, label`                |
| **bne**     | Saute si 2 registres _ne sont pas_ égaux             | I    | `bne $s, $t, label`                |
| **bgtz**    | Saute si la valeur d’un registre > 0                 | I    | `bgtz $s, label`                   |
| **blez**    | Saute si ≤ 0                                         | I    | `blez $s, label`                   |
| **div**     | Divise (signé) → résultats dans HI et LO             | R    | `div $s, $t`                       |
| **divu**    | Divise sans signe                                    | R    | `divu $s, $t`                      |
| **mult**    | Multiplie (signé) → résultats dans HI et LO          | R    | `mult $s, $t`                      |
| **multu**   | Multiplie sans signe                                 | R    | `multu $s, $t`                     |
| **mfhi**    | Copie registre HI dans un autre registre             | R    | `mfhi $d`                          |
| **mflo**    | Copie registre LO dans un autre registre             | R    | `mflo $d`                          |
| **mthi**    | Copie une valeur dans HI                             | R    | `mthi $s`                          |
| **mtlo**    | Copie une valeur dans LO                             | R    | `mtlo $s`                          |
| **j**       | Saut inconditionnel vers une adresse                 | J    | `j label`                          |
| **jal**     | Saut + sauvegarde adresse de retour                  | J    | `jal label`                        |
| **jr**      | Saut vers l’adresse contenue dans un registre        | R    | `jr $ra`                           |
| **jalr**    | Saut selon un registre et sauvegarde retour          | R    | `jalr $d, $s`                      |
| **lw**      | Charge un mot (4 octets) depuis la mémoire           | I    | `lw $t, offset($s)`                |
| **sw**      | Sauvegarde un mot dans la mémoire                    | I    | `sw $t, offset($s)`                |
| **lb**      | Charge un octet signé                                | I    | `lb $t, offset($s)`                |
| **lbu**     | Charge un octet non signé                            | I    | `lbu $t, offset($s)`               |
| **lhu**     | Charge un demi-mot non signé (2 octets)              | I    | `lhu $t, offset($s)`               |
| **lui**     | Charge une valeur dans la partie haute d’un registre | I    | `lui $t, val`                      |
| **sb**      | Sauvegarde un octet en mémoire                       | I    | `sb $t, offset($s)`                |
| **sh**      | Sauvegarde un demi-mot (2 octets)                    | I    | `sh $t, offset($s)`                |
| **sll**     | Décale à gauche (multiplie par 2ⁿ)                   | R    | `sll $d, $t, n`                    |
| **srl**     | Décale à droite logique (ajoute des 0)               | R    | `srl $d, $t, n`                    |
| **sra**     | Décale à droite arithmétique (garde le signe)        | R    | `sra $d, $t, n`                    |
| **slt**     | Met 1 si $s < $t (signé), sinon 0                    | R    | `slt $d, $s, $t`                   |
| **slti**    | Même chose mais avec valeur immédiate                | I    | `slti $t, $s, val`                 |
| **sltu**    | Compare non signé                                    | R    | `sltu $d, $s, $t`                  |
| **sltiu**   | Compare immédiate non signée                         | I    | `sltiu $t, $s, val`                |
| **or**      | OU logique entre registres                           | R    | `or $d, $s, $t`                    |
| **ori**     | OU logique avec valeur immédiate                     | I    | `ori $t, $s, val`                  |
| **nor**     | Fait le contraire d’un OU (NOT OR)                   | R    | `nor $d, $s, $t`                   |
| **xor**     | OU exclusif (XOR)                                    | R    | `xor $d, $s, $t`                   |
| **mfc0**    | Copie la valeur d’un registre du coprocesseur 0      | R    | `mfc0 $t, $c0`                     |


## Formats d'instructions MIPS

MIPS utilise **3 formats principaux** pour encoder ses instructions :

|Format|Signification|Utilisé pour|Exemple|
|---|---|---|---|
|**R** (Register)|Opérations sur **registres seulement**|add, sub, and, or, mul...|`add $t0, $t1, $t2`|
|**I** (Immediate)|Opérations avec **valeur immédiate** (constante)|addi, lw, sw, beq...|`addi $t0, $t1, 5`|
|**J** (Jump)|**Sauts inconditionnels**|j, jal|`j label`|
## Différence en pratique

## Syntaxe **add** (format R)

```
add $destination, $source1, $source2
```
- `$destination` : registre résultat (ex: `$t0`)
- `$source1`: 1er registre à additionner (ex: `$t1`)
- `$source2` : 2ème registre à additionner (ex: `$t2`)

**Exemple :**
```text
add $t0, $t1, $t2    # $t0 = $t1 + $t2
```
## Syntaxe **addi** (format I)


```text
addi $destination, $source, valeur_immediate
```

- `$destination` : registre résultat (ex: `$t0`)    
- `$source` : registre à additionner (ex: `$t1`)
- `valeur_immediate` : nombre direct (ex: `5`, `-10`)

**Exemple :**

```text
addi $t0, $t1, 100   # $t0 = $t1 + 100
```

## Tableau récapitulatif

|Instruction|Syntaxe exacte|Exemple|Résultat|
|---|---|---|---|
|**add**|`add $d, $s1, $s2`|`add $t0, $t1, $t2`|`$t0 = $t1 + $t2`|
|**addi**|`addi $d, $s, val`|`addi $t0, $t1, 42`|`$t0 = $t1 + 42`|

**Règle simple :**
- **add** = **3 registres** (registre + registre + registre)
- **addi** = **1 registre + 1 nombre** (registre + nombre + registre)
