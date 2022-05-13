# CustomEntityLoader
A plugin that simplifies the sending of `AvailableActorIdentifiersPacket` by [Rush2929](https://github.com/Rush2929)   
## download
Please note that the phar build of poggit ci is broken.  
https://github.com/DaisukeDaisukeTeam/CustomEntityLoader/releases  
## Example(and difference)
### when using CustomEntityLoader
```php
use Rush2929\CustomEntityLoader\CustomEntityLoader;
use Rush2929\CustomEntityLoader\EntityRegistryEntry;
```
```php
CustomEntityLoader::getEntityRegistry()->add(new EntityRegistryEntry(
	"Test:sample_entity", // Entity identifier
	//"minecraft:..." // behaviorId(option)
));
```
### when not using CustomEntityLoader
```php
public function onsend(DataPacketSendEvent $event): void{
    foreach($event->getPackets() as $packet){
        if(!$packet instanceof AvailableActorIdentifiersPacket) continue;
        $add = new CompoundTag();
        $add->setString("bid", "");
        $add->setByte("hasspawnegg", (int) false);
        $add->setString("id", "Test:sample_entity");
        $add->setInt("rid", 200);
        $add->setByte("summonable", (int) true);
        /** @var CompoundTag $base */
        $base = $packet->identifiers->getRoot();
        $nbt = $base->getListTag("idlist");
        if($nbt === null) throw new AssumptionFailedError("\$nbt === null");
        $nbt->push($add);
    }
}
```
```php
/*
use pocketmine\nbt\tag\CompoundTag;
use pocketmine\nbt\tag\ListTag;
use pocketmine\network\mcpe\cache\StaticPacketCache;
*/
public function onEnable(): void{
	$add = CompoundTag::create();
	$add->setString("bid", "");
	$add->setByte("hasspawnegg", (int) false);
	$add->setString("id", "test:test_entity");//entity identifier
	//$add->setInt("rid", 200);
	$add->setByte("summonable", (int) true);

	/** @var CompoundTag $nbt */
	//(StaticPacketCache)->(AvailableActorIdentifiersPacket)->(identifiers)->(CompoundTag)
	$nbt = StaticPacketCache::getInstance()->getAvailableActorIdentifiers()->identifiers->getRoot();
	/** @var ListTag $tag */
	$tag = $nbt->getListTag("idlist");
	$tag->push($add);
}
```
## credit
This repository is a fork of https://github.com/Rush2929/CustomEntityLoader.
