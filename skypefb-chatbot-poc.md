# Skype for Business Entreprise chatbot POC 
Une façon de créer un bot pour skype for business onpremise, est d'utiliser le SDK UCMA. 

UCMA est une platforme de développement qui permet de créer des applications liées au RTC Microsoft.

Les applications UCMA doivent être hébergées sur un serveur d'application.

Architecture
===
Pour développer des application UCMA, il faut obligatoirement héberger les applications dans un serveur d'application.

Les applications (bots), vont être trusted par l'environement skype. 

L'application bot va se connecter au serveur skype grâce à une adresse sip. Le bot sera ensuite présent dans la liste de contacts, comme un utilisateur lambda, ainsi tout contact skype pourra voir la présence du bot en ligne.

## Schéma

Le client et le bot passent par le front end serveur pour communiquer.

![](https://i.imgur.com/W2bkkeM.png)


Application bot console
===
:::success
Poc réussi sur UCMA 5.0 et x64
:::

## Configuration

Si vous êtes sur une machine perso (non serveur application ucma), il faudra télécharger l'API UCMA 5.0. ou (4.0 si c'est LYNC)
Dans ce sdk on retrouve un framework "BuildBot", pour pouvoir l'utiliser il faudra ajouter les références BuildABot dans notre projet visual studio (voir les section suivantes).

## Trusted application

Pour héberger votre application, il vous faudra monter/configurer un Application Server.

Ensuite, créer une trusted application et un endpoint. L'endpoint fera office de client(user). 

L'authentifiction d'application trusted permet d'accéder à certaines fonctions du serveur qui "trust", ici ça sera le FrontEnd Server qui trust l'application bot. Ce-ci afin que l'utilisateur(bot) ne se log directement sur le FrontEndServer.

Si vous avez des connaissances en systèmes/réseaux vous pouvez suivre ce [tutoriel](https://blog.thoughtstuff.co.uk/2012/12/walkthrough-creating-a-ucma-application-application-endpoint/) qui explique les étapes pour créer une trusted application et endpoint sur un environnement skype.


## Handlers Library

Dans un handler on met la logique de conversation sur un thème précis.

==HANDLER = DIALOGUE==

Lorsque le bot va recevoir un message, il va choisir quel handler est le plus approprié pour traiter le message de l'utilisateur

Il existe plusieurs types de handler, ceux qui renvoient une seul réponse, plusieurs message, ceux qui sont du type question/réponse...

### Le CODE !!!! (enfin)

#### Création d'une lib(dll) handler
Sur visual studio il faut créer un projet de type Library.

#### Ajout de références
Pour utiliser le framework BuildABot, il vous faut ajouter quelques références du framework bot dans votre projet.

Vous devez rajouter à votre solution les références suivantes :

 1. System.ComponentModel.Composition.dll.
 1. BuildABot.Core.dll
 1. BuildABot.Util.dll.

Pour voir comment trouve les BuildABot dll voir la séction

Allez dans rajouter des références, "parcourir", et recherchez vos dll dans : 

```
%ProgramFiles%\Microsoft UCMA 5.0\SDK\Core\Sample Applications\Reference\BuildABot\BuildBot.UC\bin\Debug
```

S'il ne sont pas là, vous pouvez ouvrir la solution BuildABot.UC sur visual studio -->  "Générer la solution". Ensuite vos dll seront chargés dans le dossier ci-dessus.

#### Importer les libraries 
Une fois les références ajoutés, il faut également les importer, dans votre class que vous allez créer plus bas.
```
using BuildABot.Util;
using BuildABot.Core;
using BuildABot.UC;
etc...
```


#### Creation de la classe "Echo" (handler)


```csharp==
public class Echo : SingleStateMessageHandler{
    public Echo()
        //le pattern qui active ce handler est un regex all
        :base(".*"){ 
    }
}
```


On créer un classe qui hérite de SignleStateMessageHandler (type de handler).

Le code ==.*== veut dire qu'on accepte tout type de pattern de message pour activer ce handler.


#### InitialHandle

```csharp==
protected override string GetInitialHandlingText(){
    Logger.Debug("GetInitialHandlingText function");
    return base.GetInitialHandlingText();
}
```

#### Réponse du bot
```c#==
public override Reply Handle(Message message) {
    Logger.Debug("Handle function");
    Reply reply = new Reply();
    try
    {
    reply.Add("user said : " + message.Content);
    Logger.Debug("bot message = " +    message.Content);
    }
    catch (Exception e)
    {
        Logger.Debug("error :" + e);
    }
    return reply;
           
} 

```
L'object reply est la réponse pour le client skype, on ajout un message avec la méthode Add(), ici on rajoute le message qu'on à réçu de l'utilisateur, pour faire un echo bot.


## Application console
Vous pouvez rajouter un solution à votre project. La solution sera de type "Console".
Sur votre projet vous aurez donc un solution avec deux solution, une Lib handler (PocBotLib) et un app console(LyncBotConsole).

![](https://i.imgur.com/e9aM6je.png)


### Ajouter les références 
1. Ajouter les références dll (bot framework) suivantes:
```
BuildABot.Core
BuildABot.UC
BuildABot.Util
```
![](https://i.imgur.com/fAVHCrj.png)


2. Références de vos handler
```
Soit vous ajoutez les dll qui se trouvent dans votre dossier bin de la solution lib handler. Ou alors vous pouvez aussi l'ajouter en sélectionnant une solution differectement 
```
![](https://i.imgur.com/589ttTZ.png)


3. D'autres références
```
System.ComponentModel.Composition
System.Diagnostics;
System.Configuration;
```
![](https://i.imgur.com/LNtwxFv.png)


N'oubliez pas d'importer les lib avec 'using ...'


### Class main
Votre class main doit ressembler à ça

```csharp===
static void Main(string[] args){
 
   String applicationUserAgent = ConfigurationManager.AppSettings["applicationuseragent"];
   String applicationurn = ConfigurationManager.AppSettings["applicationurn"];
   UCBotHost ucBotHost = new UCBotHost(applicationUserAgent, applicationurn);
   ucBotHost.Run();
}
```


## Déploiement 
Mettre les DLL dans le même dossier, qui va être transmit au serveur de production.

Votre dossier doit contenir: 
1. dll(s) BuildABot
2. dll(s) handlers
3. dll(s) Libs personnelles (ex:Logs, RequestApi)
3. Votre executable (console)
4. XML configuration application

> [!NOTE]
> Vous pouvez mettre ce dossier quelque part dans votre application server (ucma serveur) et exécuter votre .exe.

![](https://i.imgur.com/8fhzVFd.png)

## Déploiement avec windows service

[section : To deploy your application as a Windows service
](https://msdn.microsoft.com/fr-fr/library/office/dn454840.aspx)

https://msdn.microsoft.com/fr-fr/library/office/dn454823.aspx

## Configuration
Chose importante, il ne faut pas oublier de compiler son code pour une cible CPU x64, et sur une version .net 4.5

Version de SDK utilisé doit être la même que le serveur Skype ou Lync. Pour Skype il faut la SDK 5.0

Dans l'applicationurn et useragent, il faudra mettre l'id du trustedapplication.

Votre fichier App.config doit ressembler à ça : 
{%gist Coyla/c94d999591f3dd90a1e396bbff6124b9%}

:::info
Mettre à jour ce fichier car ce n'est pas le bon
:::


Bot consume API Web 
==

:::success
Call de skype bot --> bot cloud (bluemix) public 
réussie :+1: 
:::

Url pour appeler le bot cloud du bluemix
bot-cloud-poc.bluemixappli.staging.echonet/message
faire un POST avec un json, la réponse est un le json echo.

## RequestApi lib
On va créer une lib qui permet permet d'effectuer un request POST, qui va nous retourner un json. Pour le moment le json à envoyer va pas être dynamique, on va l'écrire en dur, seul l'url va être dynamique.

Dans notre lib qui se charge des dialogues "EchoHandler", on va rajouter les dll de la lib "RequestApi", et on va utiliser le code suivant

```javascript=
//get endpoint from app config file
String endpointBot = ConfigurationManager.AppSettings["endpoint"];
//process request
RequestApi.RequestApi request = new RequestApi.RequestApi();
String response  = request.getResponse(endpointBot);
Logger.Debug("API response : " + response);
```

## Api cloud bot
On va utiliser un module Restify pour faire tourner notre serveur web et mettre en place un système de routes. Ici on va utiliser une route ```/message``` qui va prendre un json et le renvoyer au client qui l'a envoyé.

### Installation 
```
npm install --save restify
```

### Request echo
```javascript=
server.post('/message',function(req,res,next){
    //return body(echo)
    res.send(req.body);
    return next();
});
```

## Rasa NLU - NLP API

### Installation
Le mieux c'est de l'installer depuis le git, si vous passez par la commande python, parfois il y a des fichiers qui manquent.

```
git clone https://github.com/RasaHQ/rasa_nlu.git
cd rasa_nlu
pip install -r requirements.txt
python setup.py install
```

Pour avoir toutes les dépendances liées au modules MITIE, spaCy or sklearn, ceux sont les modules de machine learning. Run la commande suivante 
```
pip install -r alt_requirements/requirements_full.txt
```

Assurez vous d'avoir ces lignes dans votre fichier requirement.txt
```
-e.
-r alt_requirements/requirements_full.txt
```

### Installation des modules 
On va utiliser le MITIE et SkLearn 
suivre le tutoriel suivant :+1: https://nlu.rasa.ai/installation.html

#### Run et test
Pour faire tourner les serveur 
```
python -m rasa_nlu.server &
```
Pour tester en envoyant un message hello, il renvoie l'intent et les entities greet.
```
$ curl -XPOST localhost:5000/parse -d '{"q":"hello there"}'
```

### Déploiement vers bluemix 
Créer une instance python et suivez les instruction pour push une application python

Avant de push il faut s'assurer d'avoir un fichier manifest.yml qui contient 
```
---
applications:
- name: rasa-nlu
  random-route : true
  memory: 128M
  command: python setup.py install --force; python -m rasa_nlu.server -P $PORT
  buildpack: python_buildpack
```

Cela sert à dire quelle type buildpack utiliser pour cloud foundry et aussi quelle command il faut lancer une fois que notre application est déployé. Ici nous demanant de lancer les commandes pour écrire notre fichier de setup ensuite, on lance le serveur nlu qui va écouter sur le même port que le serveur bluemix.

## Bot Cloud -> NLP API
:::danger
Partie en cours ... 
Faire le code pour appeler une API en POST avec json. dans la route message/
:::

## Liens 
[bot lync](https://msdn.microsoft.com/fr-fr/library/office/dn454840.aspx)
[message handlers](https://msdn.microsoft.com/fr-fr/library/office/dn454839.aspx#CreateHandlers)