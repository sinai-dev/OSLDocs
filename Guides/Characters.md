# Custom Characters

<b>Custom Characters</b> can be created and spawned with SideLoader, either through XML or C#.

The SL Pack sub-folder for custom characters is `Characters\`.
* Note: Do NOT put your Character XMLs inside a sub-folder for now, they must simply be in the Characters folder.

You can also define an SL_CharacterTrainer which is a basic pre-made trainer you can customize, and which has two additional fields: `InitialDialogue` and `SkillTree`. For details on this template, see below the first example.

See [SL_Character](API/SL_Character.md) for full documentation on this class.

## SL_Character Example

<!-- tabs:start -->

#### ** XML **

Currently, there is no debug menu generator for characters, so I'll just provide an example template here.

For SL_CharacterTrainers, see below.

```xml
<?xml version="1.0" encoding="utf-8"?>
<SL_Character xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- The UID is important! Make sure you set a unique identifier. -->
  <!-- This UID is used by OnSpawn callbacks, and can also be used for the actual Character UID. -->
  <UID>com.sinai.Fred</UID>

  <!-- One of: Temporary, Scene or Follower -->
  <SaveType>Scene</SaveType>

  <!-- Character's Name -->
  <Name>Fred</Name>
  
  <!-- Spawn information -->
  <!-- The SceneToSpawn corresponds to the scene builds names (listed on Wiki) -->
  <SceneToSpawn>ChersoneseNewTerrain</SceneToSpawn>
  <!-- Get SpawnPosition with SL Menu Tools tab, or F2 Debug Mode "Debug Position" -->
  <SpawnPosition>
    <x>133.0</x>
    <y>33.0</y>
    <z>1461.0</z>
  </SpawnPosition>
  
  <!-- AI settings. -->
  <!-- AddCombatAI will add generic combat AI to the character. -->
  <AddCombatAI>false</AddCombatAI>
  <CanDodge>true</CanDodge>
  <CanBlock>true</CanBlock>
  
  <!-- Generally use either "NONE", "Player" or "Bandits", there are other options too (Character.Factions) -->
  <Faction>Bandits</Faction>
  <!-- Human character visual data. Corresponds to Character Creation options. -->
  <CharacterVisualsData>
    <Gender>Female</Gender>
    <!-- Skin Index is ethnicity (0 aurai / 1 tramon / 2 kazite) -->
    <SkinIndex>1</SkinIndex>
    <HeadVariationIndex>3</HeadVariationIndex>
    <HairColorIndex>1</HairColorIndex>
    <HairStyleIndex>2</HairStyleIndex>
  </CharacterVisualsData>
  
  <!-- Choose the gear item IDs here -->
  <Weapon_ID>2000010</Weapon_ID>
  <!-- <Shield_ID></Shield_ID> -->
  <!-- <Helmet_ID></Helmet_ID> -->
  <!-- <Chest_ID></Chest_ID> -->
  <!-- <Boots_ID></Boots_ID> -->
  <!-- <Backpack_ID></Backpack_ID> -->
  
  <!-- Set Character Stats here -->
  <Health>100</Health>
  <HealthRegen>0</HealthRegen>
  <ImpactResist>0</ImpactResist>
  <Protection>0</Protection>
  <Damage_Resists>
    <float>1</float> <!-- phys -->
    <float>2</float> <!-- ether -->
    <float>3</float> <!-- decay -->
    <float>4</float> <!-- light -->
    <float>5</float> <!-- frost -->
    <float>6</float> <!-- fire -->
  </Damage_Resists>
  <Damage_Bonus>
    <float>1</float> <!-- phys -->
    <float>2</float> <!-- ether -->
    <float>3</float> <!-- decay -->
    <float>4</float> <!-- light -->
    <float>5</float> <!-- frost -->
    <float>6</float> <!-- fire -->
  </Damage_Bonus>
  <!-- This corresponds to Tags on statuses (eg Bleeding, Poison)... -->
  <Status_Immunity>
    <string>Bleeding</string>
    <string>Poison</string>
  </Status_Immunity>
</SL_Character>
```

#### ** C# **

In addition to the example below, there is C# summary documentation that you can see when using SideLoader methods in your IDE.

The C# classes for custom characters are `SL_Character` and `CustomCharacters`.

### OnSpawn Event

`public event Action<Character, string> OnSpawn`

The most important thing to know about your SL_Character template is the OnSpawn event.

This event is invoked <b>locally</b> for <b>all clients</b> via RPC. You do not need to use your own RPC manager for this.

When you call `CreateCharacter` manually, you can include a `string extraRpcData` with any manual network sync that you want to send.

### Examples

Here is an example from the Necromancy mod for a unique, static NPC spawn.

```csharp
private static readonly SL_Character trainerTemplate = new SL_Character()
{
    UID = TRAINER_UID,
    SaveType = CharSaveType.Scene,
    Faction = Character.Factions.NONE,
    Name = "Spectral Wanderer",
    SceneToSpawn = "Hallowed_Dungeon4_Interior",
    SpawnPosition = new Vector3(-138.3397f, 58.99699f, -102.4192f),
    Chest_ID = 3200040,
    Helmet_ID = 3200041,
    Boots_ID = 3200042,
    AddCombatAI = false,
};

// use Awake to set up Character templates.
internal void Awake()
{
    Instance = this;

    // Call this to register the internal callbacks and prepare the template.
    // For Xml templates, this is called by SLPack.LoadCharacters().
    trainerTemplate.Prepare();
    // Register our own manual spawn callback.
    trainerTemplate.OnSpawn += LocalTrainerSetup;
}

// In this case, we don't use the extraRpcData, so we can discard it with the "_" name.
public void LocalTrainerSetup(Character trainer, string _)
{
    // The Character has been created and passed to us.
    // Any local setup (like setting up NodeCanvas) can be done here.
}
```

Here is an example of a dynamic spawn from a custom skill `Effect.ActivateLocally()`.

```csharp
// The Ghost and Skeleton are SL_Character templates (with SaveType=CharSaveType.Follower), 
// didn't feel the need to show them since I showed that above.

// Setup templates in Awake
internal void Awake()
{
    Instance = this;

    Ghost.Prepare();
    Skeleton.Prepare();

    Ghost.OnSpawn += OnSpawn;
    Skeleton.OnSpawn += OnSpawn;
}

// In the custom Effect class for this "Summon" skill, 
// I override the ActivateLocally method.

// This Effect SyncType is set to MasterSync (only host executes)
public override void ActivateLocally(Character _affectedCharacter, object[] _infos)
{
    // Make a unique UID for this character, but the SL_Character template UID is used 
    // for the spawn callback.
    var summonUID = UID.Generate().ToString();

    // Get the appropriate template to use based on a custom condition check
    var template = PlagueAuraProximityCondition.InsidePlagueAura(_affectedCharacter) ? 
        Ghost : 
        Skeleton;

    var spawnPos = _affectedCharacter.transform.position + (Vector3.forward * 0.5f);

    var extraRpcData = caster.UID.ToString();

    // Call SL_Character.Spawn()
    // We are sending a spawn position, character UID, and extraRpcData.
    // In this case, the extraRpcData is the Caster character's UID.
    template.Spawn(spawnPos, summonUID, extraRpcData);
}

// The network callback from Spawn() which executes for all clients.
private void OnSpawn(Character character, string rpcData)
{
    var ownerUID = rpcData;
    var summonUID = character.UID;

    // We got the caster's UID from our extraRpcData that we sent.
    // You could use the extraRpcData string for whatever needs you have.

    // In this case, it was used to maintain a dictionary of who spawned which character.
}
```

<!-- tabs:end -->

## SL_CharacterTrainer example

For C#, it's basically the same, just see [SL_SkillTree](API/SL_SkillTree) for details on how to set the SkillTree up.

For XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<SL_CharacterTrainer xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- The UID is important! Make sure you set a unique identifier. -->
  <!-- This UID is used by OnSpawn callbacks, and can also be used for the actual Character UID. -->
  <UID>com.sinai.Fredlina</UID>

  <!-- One of: Temporary, Scene or Follower -->
  <SaveType>Scene</SaveType>

  <!-- Character's Name -->
  <Name>Fredlina</Name>
  
  <!-- Initial dialogue message when you talk to the character. -->
  <InitialDialogue>Howdy partner, wanna learn some new tricks?</InitialDialogue>
  
  <SkillTree>
	  <!-- Skill Tree UID -->
	  <UID>com.sinai.testtree</UID>
	  
	  <!-- Skill Tree Name -->
	  <Name>Test Tree</Name>
	  
	  <!-- The rows of your skill tree -->
	  <SkillRows>
	  
		<!-- First row. -->
		<SL_SkillRow>
		  <!-- Row Index can be 1 through 5, and 3 is always breakthrough -->
		  <RowIndex>1</RowIndex>
		  
		  <!-- Each row can have up to 3 slots. -->
		  <Slots>
		  
		    <!-- Each slot is an SL_BaseSkillSlot, with type SL_SkillSlot or SL_SkillSlotFork -->
			<SL_BaseSkillSlot xsi:type="SL_SkillSlot">
			
			  <!-- ColumnIndex, 1 is left, 2 is middle, 3 is right. This skill is 2 (middle) -->
			  <ColumnIndex>2</ColumnIndex>
			  
			  <!-- The x,y position of the required slot for this skill, where x is the Row and y is the ColumnIndex -->
			  <!-- 0,0 (or delete) for no requirement. -->
			  <RequiredSkillSlot>
				<x>0</x>
				<y>0</y>
			  </RequiredSkillSlot>
			  
			  <!-- The ItemID for the skill in this slot. -->
			  <SkillID>8200110</SkillID>
			  
			  <!-- Silver cost for this skill -->
			  <SilverCost>50</SilverCost>
			  
			  <!-- Is this the breakthrough? Can just delete if not -->
			  <Breakthrough>false</Breakthrough>
			  
			</SL_BaseSkillSlot>
			
			<!-- Another skill, just on the right side of the first row. -->
			<SL_BaseSkillSlot xsi:type="SL_SkillSlot">
			  <ColumnIndex>3</ColumnIndex>
			  <SkillID>8200110</SkillID>
			  <SilverCost>50</SilverCost>
			</SL_BaseSkillSlot>
			
		  </Slots>
		  
		</SL_SkillRow>
		
		<!-- Second row -->
		<SL_SkillRow>
		  <RowIndex>2</RowIndex>
		  <Slots>
		  
			<SL_BaseSkillSlot xsi:type="SL_SkillSlot">
			  <ColumnIndex>2</ColumnIndex>
			  <!-- Requires row 1, column 2 -->
			  <RequiredSkillSlot>
				<x>1</x>
				<y>2</y>
			  </RequiredSkillSlot>
			  <SkillID>8200110</SkillID>
			  <SilverCost>100</SilverCost>
			</SL_BaseSkillSlot>
			
			<SL_BaseSkillSlot xsi:type="SL_SkillSlot">
			  <ColumnIndex>3</ColumnIndex>
			  <!-- Requires row 1, column 3 -->
			  <RequiredSkillSlot>
				<x>1</x>
				<y>3</y>
			  </RequiredSkillSlot>
			  <SkillID>8200110</SkillID>
			  <SilverCost>100</SilverCost>
			</SL_BaseSkillSlot>
			
		  </Slots>
		</SL_SkillRow>
		
		<!-- Third row (Breakthrough) -->
		<SL_SkillRow>
		  <RowIndex>3</RowIndex>
		  <Slots>
			<SL_BaseSkillSlot xsi:type="SL_SkillSlot">
			  <ColumnIndex>2</ColumnIndex>
			  <RequiredSkillSlot>
				<x>2</x>
				<y>2</y>
			  </RequiredSkillSlot>
			  <SkillID>8200110</SkillID>
			  <SilverCost>500</SilverCost>
			  <!-- You don't HAVE to make this a breakthrough, so if it is a breakthrough make sure to set this to true. -->
			  <Breakthrough>true</Breakthrough>
			</SL_BaseSkillSlot>
		  </Slots>
		</SL_SkillRow>
		
		<!-- Fourth row -->
		<SL_SkillRow>
		  <RowIndex>4</RowIndex>
		  <Slots>
			<SL_BaseSkillSlot xsi:type="SL_SkillSlot">
			  <ColumnIndex>2</ColumnIndex>
			  <RequiredSkillSlot>
				<x>3</x>
				<y>2</y>
			  </RequiredSkillSlot>
			  <SkillID>8200110</SkillID>
			  <SilverCost>600</SilverCost>
			</SL_BaseSkillSlot>
		  </Slots>
		</SL_SkillRow>
		
		<!-- Fifth row (Fork) -->
		<SL_SkillRow>
		  <RowIndex>5</RowIndex>
		  <Slots>
		    <!-- Using type SL_SkillSlotFork -->
			<SL_BaseSkillSlot xsi:type="SL_SkillSlotFork">
			  <ColumnIndex>2</ColumnIndex>
			  <!-- The fork itself requires row 4, column 2 -->
			  <RequiredSkillSlot>
				<x>4</x>
				<y>2</y>
			  </RequiredSkillSlot>
			  <!-- First choice of the fork (SL_SkillSlot) -->
			  <Choice1>
				<ColumnIndex>2</ColumnIndex>
				<RequiredSkillSlot>
				  <x>4</x>
				  <y>2</y>
				</RequiredSkillSlot>
				<SkillID>8200110</SkillID>
				<SilverCost>600</SilverCost>
			  </Choice1>
			  <!-- Second choice of the fork -->
			  <Choice2>
				<ColumnIndex>2</ColumnIndex>
				<RequiredSkillSlot>
				  <x>4</x>
				  <y>2</y>
				</RequiredSkillSlot>
				<SkillID>8200110</SkillID>
				<SilverCost>600</SilverCost>
			  </Choice2>
			</SL_BaseSkillSlot>
		  </Slots>
		</SL_SkillRow>
	  </SkillRows>
  </SkillTree>

  <!-- Spawn information -->
  <!-- The SceneToSpawn corresponds to the scene builds names (listed on Wiki) -->
  <SceneToSpawn>ChersoneseNewTerrain</SceneToSpawn>
  <!-- Get SpawnPosition with SL Menu Tools tab, or F2 Debug Mode "Debug Position" -->
  <SpawnPosition>
    <x>135.0</x>
    <y>33.0</y>
    <z>1465.0</z>
  </SpawnPosition>

  <!-- AI settings. -->
  <!-- AddCombatAI will add generic combat AI to the character. -->
  <AddCombatAI>false</AddCombatAI>

  <!-- Generally use either "NONE", "Players" or "Bandits", there are other options too (Character.Factions) -->
  <Faction>NONE</Faction>
  <!-- Human character visual data. Corresponds to Character Creation options. -->
  <CharacterVisualsData>
    <Gender>Female</Gender>
    <!-- Skin Index is ethnicity (0 aurai / 1 tramon / 2 kazite) -->
    <SkinIndex>2</SkinIndex>
    <HeadVariationIndex>3</HeadVariationIndex>
    <HairColorIndex>4</HairColorIndex>
    <HairStyleIndex>5</HairStyleIndex>
  </CharacterVisualsData>

  <!-- Choose the gear item IDs here -->
  <Weapon_ID>2000010</Weapon_ID>
  <!-- <Shield_ID></Shield_ID> -->
  <!-- <Helmet_ID></Helmet_ID> -->
  <!-- <Chest_ID></Chest_ID> -->
  <!-- <Boots_ID></Boots_ID> -->
  <!-- <Backpack_ID></Backpack_ID> -->
  
</SL_CharacterTrainer>
```