- ## Activer sur le serveur (machine contrôlée)
- Sur la variante standard d'Ubuntu (avec GNOME), la fonctionnalité de partage d'écran est installée par défaut mais désactivée. Vous pouvez l'activer (et ainsi inviter une personne tierce à visionner ou à prendre le contrôle de votre ordinateur) en ouvrant les Paramètres → Partage → Bureau distant.
- Votre bureau sera en principe seulement accessible sur le réseau local. Il faudra si besoin ouvrir le port 3389 de votre routeur pour le rendre accessible par Internet. Assurez-vous évidemment d'entrer un mot de passe pour assurer la sécurité sur votre réseau local et d'autant plus fort pour Internet.
- Le partage de bureau ne sera activé que pendant la durée de votre session, ou jusqu'à ce que vous le désactiviez.
- Pour information le logiciel fournissant cette fonctionnalité s'appelle gnome-remote-dekstop et utilise le protocole RDP. On peut choisir d'utiliser en plus "l'ancien protocole VNC".
- ### Activer via SSH
- Se connecter via SSH et :
  ```bash
  export DISPLAY=localhost:0.0
  sudo apt-get install dbus-x11
  grdctl --help
  grdctl rdp set-credentials <username> <password>
  grdctl rdp enable
  ```
- Activer / désactiver le contrôle à distance
  ```bash
  grdctl rdp disable-view-only
  grdctl rdp enable-view-only
  ```
- ## Côté client
- Utiliser remmina
- Les fichiers des configurations créées sont dans `~/.local/share/remmina/`.
- On peut les lancer en ligne de commande
  ```bash
  remmina -c ~/.local/share/remmina/<File>.remmina
  ```
-
- ## autres
- Voir les machines sur le réseaux local
  ```bash
  arp -a
  ```
- Voir leur nom mDNS
  ```bash
  avahi-resolve-address -a 192.168.246.211
  ```
- ## Sources
- https://doc.ubuntu-fr.org/remmina#lancement_de_l_application