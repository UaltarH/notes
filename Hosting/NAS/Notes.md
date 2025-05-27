# Accès extérieur

## SMB

Le protocol `SMB` présente l'avantage de supporter la lecture des fichiers volumineux contrairement au protocole `WebDAV`.

1. Activer le service SMB dans `Services de fichiers`
2. Créer un DDNS dans `Accès externe`  ^6ba7e5
3. Installer `VPN Server` depuis le `Centre de paquets`
4. ![[openVPN_config.png]]
5. Exporter la configuration
6. Editer la config et insérer le nom de domaine créé dans la partie précédente [[Hosting/NAS/Notes#^6ba7e5|DDNS]]	
7. Utilisation
	1. Windows :
		- Si vous utilisez OpenVPN community, copier la config `VPNConfig.ovpn` dans `C:\Program Files\OpenVPN\config`
		- Installer OpenVPN connect ou community et charger la config
		- Dans l'explorateur de fichier "Connecter un lecteur réseau" 
		- Dans `Adresse réseau ou Internet`, spécifier `\\192.168.XX.XX\<shared folder>`
	2. MacOs :
		- Télécharger OpenVPN connect et charger la config.
		- Sur `Finder` > `Aller`> `Se connecter au serveur ...` 
		- Ajouter l'adresse : `smb://192.168.XX.XX/<shared folder>`
## NFS

1. Activer le service NFS dans `Services de fichiers`
2. Dans `Dossier partagé`> `Modifier` > `Autorisations NFS`
3. Créer une entrée, dans `nom d'hôte ou IP` vous pouvez préciser l'IP de l'hôte sur lequel vous voulez accéder au NAS, ou vous pouvez directement l'exposer dans le sous réseau entier en mettant : `192.168.X.0/24`

## Proxmox
[[Hosting/Proxmox/Notes|Notes]]

## Backup
Utiliser hyper backup en roulement, backup 1 service par jour la nuit