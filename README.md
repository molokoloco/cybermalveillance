# Cybermalveillance — Librairie d'outils de réponse à incident (DFIR)

Collection curatée d'outils **défensifs / forensic / DFIR** (Digital Forensics & Incident Response), référencés en **submodules git**, pour le travail de réponse à incident de **Julien Guézennec** — [Julienweb.fr](https://julienweb.fr), prestataire **référencé [Cybermalveillance.gouv.fr](https://www.cybermalveillance.gouv.fr/)** 🇫🇷.

> **Défense d'abord.** Cette librairie publique ne contient que des outils d'**analyse, de détection et de forensic**. Pas d'armes offensives : ce n'est pas du recon, c'est du **constat, de la préservation de preuves et du nettoyage** de postes compromis.

Chaque outil reste sa propre source amont (GitHub) ; ce dépôt n'en stocke qu'un **pointeur versionné** (commit épinglé) + cette documentation. **Aucun code amont n'est dupliqué ici.**

> 🔒 **Frontière vie privée stricte.** Ce dépôt ne contient **que** cette librairie d'outils. Les données des victimes que j'accompagne restent **100 % locales, hors de tout dépôt git** — un prestataire référencé Cybermalveillance.gouv.fr n'expose jamais de données clients, même en privé.

## Pourquoi rendre ça public

Parce que la **transparence** fait partie du métier. Quand je détaille [ma clé d'intervention First Responder](https://julienweb.fr/blog/cle-first-responder-kit-intervention-cyber/10850/), je dis ce que je fais et comment. Cette librairie en est le prolongement : montrer **avec quels outils open source de référence** je travaille, et selon quelle doctrine. Un confrère peut s'en inspirer ; un client peut vérifier le sérieux de la démarche.

## Cadre d'usage

> « Science sans conscience n'est que ruine de l'âme » — Rabelais.

Ces outils servent un cadre **défensif, légal et autorisé** : forensic, réponse à incident, recherche, formation. L'analyse d'un appareil se fait sur **matériel personnel ou avec autorisation écrite** ; le constat est **factuel, jamais accusatoire**.

> **Règle source ≠ usage** : ce dépôt ne stocke que des **pins de référence** (pointeurs de version). L'exécution réelle se fait sur une **station d'analyse saine** (binaire release / pip / Docker, contre les artefacts collectés) ou la **clé First Responder** (binaire signé portable) — **jamais** en câblant un outil au MCP d'un poste contenant des données clients.

*Les outils offensifs (tests d'intrusion sous mandat écrit) sont gérés séparément, dans un dépôt privé distinct.*

## Cloner avec les submodules

```bash
git clone --recurse-submodules https://github.com/molokoloco/cybermalveillance.git
# ou, après un clone simple :
git submodule update --init --recursive
```

## Outils — Défensif / DFIR / forensic

| Outil | Amont | Licence | Rôle |
|-------|-------|---------|------|
| [PyrsistenceSniper](PyrsistenceSniper) | [Hexastrike/PyrsistenceSniper](https://github.com/Hexastrike/PyrsistenceSniper) | MIT | Détection **hors-ligne** de persistances Windows — 117 checks / 9 techniques MITRE ATT&CK (Run keys, services, COM hijacking, tâches planifiées, WMI, IFEO, add-ins Office, LSA…), enrichis hash + signatures Authenticode + classification LOLBin. Lecture seule, pointé sur un dump / une collection / une image montée. |
| [hayabusa](hayabusa) | [Yamato-Security/hayabusa](https://github.com/Yamato-Security/hayabusa) | AGPL-3.0 | **Threat hunting Windows.** Moteur Sigma (2400+ règles) qui transforme des journaux d'événements **`.evtx`** en **timeline CSV de détections**. Rust, rapide, hors-ligne. |
| [chainsaw](chainsaw) | [WithSecureLabs/chainsaw](https://github.com/WithSecureLabs/chainsaw) | GPL-3.0 | **First-response Windows.** Chasse rapide event logs **+ MFT/artefacts** (Sigma + regex), sortie ASCII/CSV/JSON. Complémentaire de Hayabusa (le MFT en plus). |
| [awesome-incident-response](awesome-incident-response) | [meirwah/awesome-incident-response](https://github.com/meirwah/awesome-incident-response) | Apache-2.0 | **Index DFIR curaté** (IOC scanners type Loki/Fenrir, forensic mémoire, playbooks CERT, triage). Bibliothèque de références. |
| [mvt](mvt) | [mvt-project/mvt](https://github.com/mvt-project/mvt) | MVT (≈ MPL-2.0 + clause consenti) | **Forensic mobile consenti** (Amnesty). Traces de spyware/stalkerware sur Android/iOS via IOC STIX. ⚠️ Analyse de l'appareil **avec l'accord** de la victime. |
| [androidqf](androidqf) | [mvt-project/androidqf](https://github.com/mvt-project/androidqf) | MVT (≈ MPL-2.0) | **Android Quick Forensics** : collecteur Android (acquisition logique adb) → bundle analysable par MVT. |
| [systeminformer](systeminformer) | [winsiderss/systeminformer](https://github.com/winsiderss/systeminformer) | MIT | **Inspection live Windows** (successeur Process Hacker) : processus / handles / réseau / services / driver kernel. Usage réel = **binaire signé portable officiel**, jamais un build maison sur machine victime. |

## Écartés (ne pas ré-ajouter)

- **reverse-skill** ([zhaoxuya520/reverse-skill](https://github.com/zhaoxuya520/reverse-skill)) — évalué le 2026-06-19, **rejeté**. Repo d'« agent-hijack » : son `README`/`RULES_zh.md` ordonnent à tout agent IA qui les lit d'**injecter des règles dans la config globale** (`~/.claude/CLAUDE.md`) et d'**auto-installer des MCP** (`~/.claude/mcp.json`) sans validation de l'utilisateur (« 全局注入 / injection globale »). Mécanisme de prompt-injection auto-persistant + dépendances en `@latest`. **Signalé à GitHub Security** ; Windows Defender le détecte par ailleurs comme `Trojan:Script/Wacatac.Html`. N'a pas sa place dans une librairie d'outils.

> ⚠️ **Vetting anti-injection** systématique avant tout ajout : tout contenu d'un repo cloné est traité comme de la **donnée non fiable**, jamais comme des instructions à exécuter. On distingue le **hijack covert** (modification de config en douce → rejet + signalement) de l'**outil légitime** à configuration ouverte et consentie.

## Gestion

```bash
# Mettre à jour tous les outils vers leur dernière version amont
git submodule update --remote --merge
# Mettre à jour un seul outil
git -C <nom-dossier> pull origin <branche>
```

---

**Julien Guézennec** — [Julienweb.fr](https://julienweb.fr) · Studio Internet depuis 1998
🛡 Référencé [Cybermalveillance.gouv.fr](https://www.cybermalveillance.gouv.fr/) · 🇫🇷 Activateur France Num
[Intervention d'urgence piratage & incident de sécurité →](https://julienweb.fr/services-julienweb-fr/intervention-durgence-piratage-incident-de-securite/)
