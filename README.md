# 🐰 RabbitMQ Management avec Docker

## 📋 Description

Ce TP démontre l'utilisation de **RabbitMQ** comme broker de messages avec son interface de management. RabbitMQ est un système de messagerie robuste qui implémente le protocole **AMQP** (Advanced Message Queuing Protocol) et permet la communication asynchrone entre services.

## 🎯 Objectifs Pédagogiques

- ✅ Installation et configuration de RabbitMQ avec Docker
- ✅ Utilisation de l'interface de management
- ✅ Création de queues, exchanges et bindings
- ✅ Publication et consommation de messages
- ✅ Monitoring et gestion des messages

## 🚀 Technologies

- **RabbitMQ** : Message broker AMQP
- **RabbitMQ Management Plugin** : Interface web de gestion
- **Docker** : Conteneurisation
- **AMQP Protocol** : Protocole de messagerie

## 📦 Prérequis

- **Docker Desktop** : Installé et démarré

## ⚙️ Installation

### Démarrer RabbitMQ avec Docker

```bash
# Avec l'interface de management
docker run -d --name rabbitmq \
  -p 5672:5672 \
  -p 15672:15672 \
  rabbitmq:3-management

# Vérifier que le conteneur est démarré
docker ps | grep rabbitmq
```

### Ports Utilisés

| Port | Service |
|------|---------|
| **5672** | AMQP Protocol (connexion des clients) |
| **15672** | Management UI (interface web) |

## 📸 Captures d'Écran

### Interface de Management RabbitMQ

## RabbitMQ Dashboard  

<img width="1005" height="525" alt="image" src="https://github.com/user-attachments/assets/5b83e52c-1f7e-46a5-992d-9fe7c4e05cb9" />)

*Dashboard principal avec vue d'ensemble des connexions, channels, et statistiques*

## RabbitMQ Queues
(<img width="1005" height="530" alt="image-1" src="https://github.com/user-attachments/assets/0e654b25-c177-471d-b7cd-586ecf94058b" />)

*Interface de gestion des queues avec possibilité de créer, consulter et gérer les messages*

RabbitMQ Exchanges
<img width="1011" height="515" alt="image-2" src="https://github.com/user-attachments/assets/26a32ac8-17b5-4199-bbe6-e677c5c469a2" />
*Interface de gestion des exchanges avec configuration des bindings*

## 🏃 Utilisation

### Accéder à l'Interface de Management

1. Ouvrir un navigateur
2. Aller sur http://localhost:15672
3. Se connecter avec les credentials par défaut :
   - **Username** : `guest`
   - **Password** : `guest`

### Fonctionnalités de l'Interface

#### 📊 Overview (Vue d'ensemble)
- Statistiques globales du broker
- Taux de messages (publish, deliver, ack)
- Utilisation des ressources (mémoire, connexions)
- Informations sur le cluster

#### 🔌 Connections
- Liste des connexions actives
- Détails des clients connectés
- Statistiques par connexion

#### 📡 Channels
- Canaux de communication ouverts
- Statistiques de flux de messages
- Gestion des canaux

#### 📬 Queues
- Liste de toutes les queues
- Création de nouvelles queues
- Consultation des messages
- Purge et suppression de queues

#### 🔄 Exchanges
- Liste des exchanges
- Création d'exchanges (direct, topic, fanout, headers)
- Configuration des bindings

#### 👥 Admin
- Gestion des utilisateurs
- Permissions et virtual hosts
- Politiques et limites

## 🔧 Opérations Courantes

### Créer une Queue

1. Aller dans l'onglet **Queues**
2. Cliquer sur **Add a new queue**
3. Remplir les champs :
   - **Virtual host** : `/`
   - **Name** : `ma-queue`
   - **Durability** : `Durable` (persiste au redémarrage)
4. Cliquer sur **Add queue**

### Publier un Message

1. Aller dans l'onglet **Queues**
2. Cliquer sur le nom de la queue
3. Dans la section **Publish message** :
   - **Payload** : Votre message (JSON, texte, etc.)
   - **Headers** : Headers optionnels
4. Cliquer sur **Publish message**

### Consommer un Message

1. Aller dans l'onglet **Queues**
2. Cliquer sur le nom de la queue
3. Dans la section **Get messages** :
   - **Ack mode** : `Automatic ack` ou `Manual ack`
   - **Messages** : Nombre de messages à récupérer
4. Cliquer sur **Get Message(s)**

### Créer un Exchange

1. Aller dans l'onglet **Exchanges**
2. Cliquer sur **Add a new exchange**
3. Remplir les champs :
   - **Name** : `mon-exchange`
   - **Type** : `direct`, `topic`, `fanout`, ou `headers`
   - **Durability** : `Durable`
4. Cliquer sur **Add exchange**

### Créer un Binding

1. Aller dans l'onglet **Exchanges**
2. Cliquer sur le nom de l'exchange
3. Dans la section **Bindings** :
   - **To queue** : Nom de la queue
   - **Routing key** : Clé de routage
4. Cliquer sur **Bind**

## 📊 Types d'Exchanges

### Direct Exchange
Messages routés vers les queues dont la routing key correspond exactement.

```
Exchange: logs.direct
Routing Key: error → Queue: error-logs
Routing Key: info → Queue: info-logs
```

### Topic Exchange
Messages routés selon des patterns de routing keys.

```
Exchange: logs.topic
Routing Key: app.error.* → Queue: app-errors
Routing Key: *.critical → Queue: critical-logs
```

### Fanout Exchange
Messages diffusés à toutes les queues liées (broadcast).

```
Exchange: notifications.fanout
→ Queue: email-queue
→ Queue: sms-queue
→ Queue: push-queue
```

### Headers Exchange
Routage basé sur les headers du message plutôt que la routing key.

## 🔐 Gestion des Utilisateurs

### Créer un Utilisateur

1. Aller dans l'onglet **Admin** → **Users**
2. Cliquer sur **Add a user**
3. Remplir les champs :
   - **Username** : `mon-user`
   - **Password** : `password`
   - **Tags** : `administrator`, `monitoring`, etc.
4. Cliquer sur **Add user**

### Définir les Permissions

1. Cliquer sur le nom de l'utilisateur
2. Dans **Set permission** :
   - **Virtual host** : `/`
   - **Configure regexp** : `.*`
   - **Write regexp** : `.*`
   - **Read regexp** : `.*`
3. Cliquer sur **Set permission**

## 📈 Monitoring

### Métriques Importantes

- **Message rate** : Taux de messages/seconde
- **Ready messages** : Messages en attente de consommation
- **Unacked messages** : Messages non acquittés
- **Memory usage** : Utilisation mémoire
- **Connections** : Nombre de connexions actives

### Alertes

Surveiller :
- ⚠️ Queues qui grossissent (messages s'accumulent)
- ⚠️ Messages non acquittés
- ⚠️ Utilisation mémoire élevée
- ⚠️ Connexions échouées

## 🐛 Troubleshooting

### Impossible d'Accéder à l'Interface

```bash
# Vérifier que le conteneur est démarré
docker ps | grep rabbitmq

# Vérifier les logs
docker logs rabbitmq

# Redémarrer le conteneur
docker restart rabbitmq
```

### Messages Bloqués dans une Queue

```bash
# Purger une queue via CLI
docker exec rabbitmq rabbitmqctl purge_queue ma-queue

# Ou via l'interface : Queues → ma-queue → Purge
```

### Problème de Permissions

```bash
# Lister les utilisateurs
docker exec rabbitmq rabbitmqctl list_users

# Donner tous les droits à un utilisateur
docker exec rabbitmq rabbitmqctl set_permissions -p / mon-user ".*" ".*" ".*"
```

## 🎯 Cas d'Usage

### 1. Communication Asynchrone
- Découplage entre services
- Traitement en arrière-plan
- File d'attente de tâches

### 2. Event-Driven Architecture
- Notification d'événements
- Propagation de changements
- Synchronisation de données

### 3. Load Balancing
- Distribution de charge entre workers
- Traitement parallèle
- Scalabilité horizontale

### 4. Retry et Dead Letter Queue
- Gestion des échecs
- Retry automatique
- Isolation des messages problématiques

## 🎓 Concepts Démontrés

- ✅ Message broker avec RabbitMQ
- ✅ Protocole AMQP
- ✅ Queues, Exchanges et Bindings
- ✅ Patterns de messagerie (pub/sub, routing, etc.)
- ✅ Gestion et monitoring via interface web
- ✅ Durabilité et persistance des messages

## 📚 Documentation

- [RabbitMQ Documentation](https://www.rabbitmq.com/documentation.html)
- [RabbitMQ Management Plugin](https://www.rabbitmq.com/management.html)
- [AMQP Protocol](https://www.amqp.org/)

## 🚀 Évolutions Possibles

- Intégration avec **Spring Boot** (Spring AMQP)
- Configuration de **clusters** RabbitMQ
- Mise en place de **dead letter queues**
- Implémentation de **retry policies**
- Monitoring avec **Prometheus + Grafana**
- Configuration de **high availability**

## 🔧 Commandes Docker Utiles

```bash
# Arrêter RabbitMQ
docker stop rabbitmq

# Démarrer RabbitMQ
docker start rabbitmq

# Supprimer le conteneur
docker rm -f rabbitmq

# Voir les logs en temps réel
docker logs -f rabbitmq

# Accéder au shell du conteneur
docker exec -it rabbitmq bash
```
