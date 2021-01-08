# Custom Skill Trees

Custom Skill Trees can be defined through C# or with XML (through an SL_CharacterTrainer).

See [SL_SkillTree](API/SL_SkillTree.md) for full documentation.

## Examples

<!-- tabs:start -->

#### ** XML **

See [Custom Characters](Guides/Characters.md), and use an SL_CharacterTrainer template.

#### ** C# **

This is an example from the Necromancy mod, showing how to define a Skill Tree.

Note that this does not set up the trainer, see [Custom Characters](Guides/Characters.md) for that.

```csharp
// namespace .. class ..

// You could keep a reference to your SkillSchool if you need it
public static SkillSchool MyTree;

// in your Awake method:
internal void Awake()
{
	// Do this after OnPacksLoaded, and set up your
	// custom skills before this point (Awake or BeforePacksLoaded).
	SL.OnPacksLoaded += Setup;
}

private void Setup()
{
	// Call this first, to create the SkillSchool and register it to the game's SkillTreeHolder
	MyTree = Template.CreateBaseSchool(true);
}

private static readonly SL_SkillTree Template = new SL_SkillTree()
{
	Name = "Necromancy",
	SkillRows = new List<SL_SkillRow>()
	{
		new SL_SkillRow()
		{
			RowIndex = 1,
			Slots = new List<SL_BaseSkillSlot>()
			{
				new SL_SkillSlot() // Summon
				{
					ColumnIndex = 2,
					SilverCost = 50,
					SkillID = 8890103,
				},
				new SL_SkillSlot() // Vital Attunement
				{
					ColumnIndex = 3,
					SilverCost = 50,
					SkillID = 8890101,
				},
			}
		},
		new SL_SkillRow()
		{
			RowIndex = 2,
			Slots = new List<SL_BaseSkillSlot>()
			{
				new SL_SkillSlot() // Life Ritual
				{
					ColumnIndex = 2,
					SkillID = 8890105,
					SilverCost = 100,
					RequiredSkillSlot = new Vector2(1, 2), // requires Summon (row 1, slot 2)
				},
				new SL_SkillSlot() // Tendrils
				{
					ColumnIndex = 3,
					SkillID = 8890100,
					SilverCost = 100,
					RequiredSkillSlot = new Vector2(1, 3), // requires Vital Attunement (row 1, slot 3)
				}
			}
		},
		new SL_SkillRow()
		{
			RowIndex = 3,
			Slots = new List<SL_BaseSkillSlot>() // Transcendence (Breakthrough)
			{
				new SL_SkillSlot()
				{
					Breakthrough = true,
					SkillID = 8890104,
					RequiredSkillSlot = new Vector2(2, 2), // requires Life Ritual (row 2, slot 2),
					SilverCost = 500,
					ColumnIndex = 2
				}
			}
		},
		new SL_SkillRow()
		{
			RowIndex = 4,
			Slots = new List<SL_BaseSkillSlot>() // Death Ritual
			{
				new SL_SkillSlot()
				{
					ColumnIndex = 2,
					RequiredSkillSlot = new Vector2(3, 2), // requires breakthrough
					SkillID = 8890106,
					SilverCost = 600,
				}
			}
		},
		new SL_SkillRow()
		{
			RowIndex = 5,
			Slots = new List<SL_BaseSkillSlot>() // fork choice
			{
				new SL_SkillSlotFork()
				{
					ColumnIndex = 2,
					RequiredSkillSlot = new Vector2(4, 2), // requires Death Ritual
					Choice1 = new SL_SkillSlot() // Plague Aura
					{
						ColumnIndex = 2,
						SilverCost = 600,
						SkillID = 8890107,
						RequiredSkillSlot = new Vector2(4, 2), // requires Death Ritual
					},
					Choice2 = new SL_SkillSlot() // Army of Death
					{
						ColumnIndex = 2,
						SilverCost = 600,
						SkillID = 8890108,
						RequiredSkillSlot = new Vector2(4, 2), // requires Death Ritual
					}
				}
			}
		}
	}
};
```

<!-- tabs:end -->