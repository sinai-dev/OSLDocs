# Custom Characters

<b>Custom Characters</b> can be created and spawned with SideLoader, either through XML or C#.

The SL Pack sub-folder for custom characters is `Characters\`.
* Note: Do NOT put your Character XMLs inside a sub-folder for now, they must simply be in the Characters folder.

You can also define an SL_CharacterTrainer which is a basic pre-made trainer you can customize, and which has two additional fields: `InitialDialogue` and `SkillTree`. For details on this template, see below the first example.

See [SL_Character](API/SL_Character.md) for full documentation on this class.

If you wanted to spawn the characters from an effect on a weapon or skill, you can use [SL_SpawnSLCharacter](SL_Effect?id=sl_spawnslcharacter-sl_effect).

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

  <!-- Scene-Spawn information -->
  <!-- The SceneToSpawn corresponds to the scene builds names (listed on Wiki and SL Menu) -->
  <SceneToSpawn>ChersoneseNewTerrain</SceneToSpawn>
  
  <!-- Used if SceneToSpawn is set. -->
  <!-- Get SpawnPosition with SL Menu Tools tab, or F2 Debug Mode "Debug Position" -->
  <SpawnPosition>
    <x>133.0</x>
    <y>33.0</y>
    <z>1461.0</z>
  </SpawnPosition>
  <SpawnRotation>
    <x>0</x>
    <y>0</y>
    <z>0</z>
  </SpawnRotation>
  
  <DestroyOnDeath>false</DestroyOnDeath>
  
  <!-- Generally use either "NONE", "Player" or "Bandits", there are other options too (Character.Factions) -->
  <Faction>Bandits</Faction>
  
  <!-- If you want to set manual targetable factions, uncomment and set them here. -->
  <!-- <TargetableFactions>
	<Factions>Player</Factions>
	<!-- etc... -->
  </TargetableFactions> -->
  
  <!-- Human character visual data. Corresponds to Character Creation options. -->
  <CharacterVisualsData>
    <Gender>Female</Gender>
    <!-- Skin Index is ethnicity (0 aurai / 1 tramon / 2 kazite, -999 trog) -->
    <SkinIndex>1</SkinIndex>
    <HeadVariationIndex>3</HeadVariationIndex>
    <HairColorIndex>1</HairColorIndex>
    <HairStyleIndex>2</HairStyleIndex>
  </CharacterVisualsData>

  <!-- Choose the gear item IDs here -->
  <Weapon_ID>2000010</Weapon_ID>
  <Shield_ID>-1</Shield_ID>
  <Helmet_ID>-1</Helmet_ID>
  <Chest_ID>-1</Chest_ID>
  <Boots_ID>-1</Boots_ID>
  <Backpack_ID>-1</Backpack_ID>

  <!-- Set Character Stats here -->
  <Health>100</Health>
  <HealthRegen>0</HealthRegen>
  <ImpactResist>0</ImpactResist>
  <Protection>0</Protection>
  <Barrier>0</Barrier>
  
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
  
  <!-- the AI is not mandatory, you can delete the <AI> completely to have no AI. -->
  <AI xsi:type="SL_CharacterAIMelee">
  
	<!-- Basic settings -->
    <CanDodge>true</CanDodge>
    <CanBlock>true</CanBlock>
    <CanWanderFar>true</CanWanderFar>
    <ForceNonCombat>false</ForceNonCombat>
	<!-- Not sure if functional yet, for "contagious" AI behaviour -->
    <AIContagionRange>20</AIContagionRange>
	
	<!-- Wander State settings -->
    <Wander_Speed>1.1</Wander_Speed>  <!-- move speed -->
    <Wander_FollowPlayer>false</Wander_FollowPlayer>
	
	<!-- Wander Type, one of: Wander or Patrol -->
	<!-- "Wander" is just free roam, "Patrol" requires you to set the Waypoints and uses those -->
    <Wander_Type>Patrol</Wander_Type>
	
	<!-- Waypoints for Patrol mode. -->
    <Wander_PatrolWaypoints>
	
	  <!-- Define as many SL_Waypoint objects as you want. -->
      <SL_Waypoint>
	    <!-- World position of waypoint -->
        <WorldPosition>
          <x>133.0</x>
          <y>33.0</y>
          <z>1461.0</z>
        </WorldPosition>
		<!-- Random position offset -->
        <RandomRadius>15</RandomRadius>
        <!-- Time to stay at waypoint (random min max) -->
		<WaitTime>
          <x>5</x>
          <y>10</y>
        </WaitTime>
      </SL_Waypoint>
	  
      <SL_Waypoint>
        <WorldPosition>
          <x>153.0</x>
          <y>33.0</y>
          <z>1491.0</z>
        </WorldPosition>
        <RandomRadius>15</RandomRadius>
        <WaitTime>
          <x>4</x>
          <y>15</y>
        </WaitTime>
      </SL_Waypoint>
	  
	  <!-- etc... -->
    </Wander_PatrolWaypoints>
	
	<!-- Suspicious state settings -->
    <Suspicious_Speed>1.75</Suspicious_Speed> <!-- move speed -->
    <Suspicious_Duration>5</Suspicious_Duration>
    <Suspicious_Range>30</Suspicious_Range>
    <Suspicious_TurnModif>0.9</Suspicious_TurnModif>
	
	<!-- Combat state settings -->
	<!-- I haven't looked too deeply into what all these do yet, experiment and play around. -->
    <Combat_ChargeTime>
      <x>4</x>
      <y>8</y>
    </Combat_ChargeTime>
    <Combat_TargetVulnerableChargeModifier>0.5</Combat_TargetVulnerableChargeModifier>
    <Combat_ChargeAttackRangeMulti>1</Combat_ChargeAttackRangeMulti>
    <Combat_ChargeTimeToAttack>0.4</Combat_ChargeTimeToAttack>
    <Combat_ChargeStartRangeMult>
      <x>0.8</x>
      <y>3</y>
    </Combat_ChargeStartRangeMult>
    <Combat_SpeedModifiers>
      <float>0.8</float>
      <float>1.3</float>
      <float>1.7</float>
    </Combat_SpeedModifiers>
    <Combat_ChanceToAttack>75</Combat_ChanceToAttack>
    <Combat_KnowsUnblockable>true</Combat_KnowsUnblockable>
    <Combat_DodgeCooldown>3</Combat_DodgeCooldown>
	<!-- The AttackPatterns are the melee attacks the character can perform. -->
    <Combat_AttackPatterns>
	
	  <!-- Define as many AttackPattern objects as you like -->
	  <!-- Make sure to set the ID to a unique number for each pattern. -->
      <AttackPattern>
        <ID>0</ID>
		<!-- Chance weight -->
        <Chance>20</Chance>
        <Range>
          <x>0.9</x>
          <y>2.5</y>
        </Range>
		<!-- The actual attacks for this pattern. -->
		<!-- Define as many AtkTypes entries as you want. -->
		<!-- An AtkTypes can be Normal or Special. -->
        <Attacks>
          <AtkTypes>Normal</AtkTypes>
        </Attacks>
		<!-- Required HP percent min and max -->
        <HPPercent>
          <x>-1</x>
          <y>-1</y>
        </HPPercent>
      </AttackPattern>
	  
      <!-- etc, define as many as you want... -->
	  
    </Combat_AttackPatterns>
	
  </AI> <!-- If not using AI at all, remove everything from first <AI> to here. -->
  
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
// You could also just use an SL_CharacterTrainer, but this example was made
// before that was developed.
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
	// etc...
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

You could also look at the NecromancySkills' [SummonManager.cs](https://github.com/sinai-dev/Outward-Mods/blob/master/Necromancy/src/SummonManager.cs) and [SummonSkeleton.cs](https://github.com/sinai-dev/Outward-Mods/blob/master/Necromancy/src/CustomSkills/SummonSkeleton.cs) classes for some practical examples.

<!-- tabs:end -->

## SL_CharacterTrainer example

For C#, it's basically the same, just see [Custom Skill Trees](Guides/SkillTrees.md) for details on how to set the SkillTree up.

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

  <!-- Spawn information -->
  <!-- The SceneToSpawn corresponds to the scene builds names (listed on Wiki) -->
  <SceneToSpawn>ChersoneseNewTerrain</SceneToSpawn>
  <!-- Get SpawnPosition with SL Menu Tools tab, or F2 Debug Mode "Debug Position" -->
  <SpawnPosition>
    <x>135.0</x>
    <y>33.0</y>
    <z>1465.0</z>
  </SpawnPosition>
  
  <SpawnRotation>
	<x>0.0</x>
	<y>0.0</y>
	<z>0.0</z>
  </SpawnRotation>

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
  
</SL_CharacterTrainer>
```