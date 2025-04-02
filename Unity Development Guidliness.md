# Memoria Coding Guidelines

**Table of contents**

- [Git](#git)
  - [Cloning the solution](#cloning-the-solution)
  - [Creating the Unity folder structure from scratch](#creating-the-unity-folder-structure-from-scratch)

- [Folder and File naming](#folder-and-file-naming)
  - [Script naming](#script-naming)

- [General coding guidelines](#general-coding-guidelines)
  - [Code formatting](#code-formatting)
  - [Variable naming](#variable-naming)
  - [Member Properties (getter/setter)](#member-properties-gettersetter)
  - [Attributes](#attributes)
  - [Functions / Methods](#functions-methods)
  - [Events and Subscriptions](#events-and-subscriptions)
  - [Comments](#comments)
  - [Enums](#enums)
  - [Code order](#code-order)

- [Avoid this](#avoid-this)

## Git
### Cloning the solution
1. Create one folder with name of the project. Example: `project-recoil`.
2. Create a local git repository in this folder and clone the remote Git repo there.

### Creating the Unity folder structure from scratch
1. Create following folder structure: 
  > - `project-recoil` Name of your game. Separated by dash if contains multiple words. Word project can be used too when final name of the game is not known. 
  >   - `art` Contains raw art from Blender, Photoshop, etc. Feel free to create sub-folders as you want. This IS NOT stored in Git.
  >   - `build` Contains builds created by Unity. This is not stored in Git.
  >   - `devlog` Contains videos and screenshots from the development. This IS NOT stored in Git. 
  >   - `marketing-materials` Contains materials like PNG with latest game update. Banners. Logos. This IS stored in Git.
  >   - `unity-project` This is where the Unity files will be stored.
  >     - `project-recoil` Again name of your game - you don't want to name it differently as this is what you going to see on your builds/.exe file.
  >       - `Assets` Following structure is a must for each 3D project.
  >         - `Animations`
  >         - `Editor`
  >         - `Fonts`
  >         - `Materials`
  >         - `Memoria`
  >         - `Models`
  >         - `Prefabs`
  >         - `Resources`
  >         - `Sandbox`
  >         - `Scenes`
  >         - `Scripts`
  >         - `Settings`
  >         - `Sounds`
  >         - `Textures`
  >         - `ThirdParty`

2. Add `.gitignore` to your root project folder (the very first `project-recoil` folder):
  ```gitignore
  art/
  build/
  devlog/
  unity-project/project-recoil/[Ll]ibrary/
  unity-project/project-recoil/[Tt]emp/
  ```
3. Add empty `.gitkeep` file to each folder your want to push to Git even when the folder is empty. Better to do it with all Unity folders like `Animations`, `Models`, etc. and `marketing-materials`.
4. Create remote repository without the `readme.txt` file.
4. Push your local repository to the remote repository.

### Git best practices

[//]: # (TODO: Add best practices once they are discussed with the team.)

## Folder and File naming
- Use Pascal Case for `FolderName`, `ThirdParty` `Scripts` `FireDragon`.
- Use Pascal Case except for images: `ScriptName.cs` `PrefabName.cs` `SceneName.cs` `image-name-64x64.jpg`.

[//]: # (TODO: Unity Object naming)

### Script naming
- UI scripts use `MenuPanel` `AchievementSlot` `CoinShopButton`.
- Master scripts that control specific workflow (only ONE instance in the scene): `SoundManager` `AchievementManager`.
- For scripts controlling a game object (one or many in the scene): `PlayerController` `BossController` `BackgroundController`.
- For a database (e.g CSV, JSON) which contains a list of data: `WeaponDatabase` `CardDatabase`.
- For Object in Database: `WeaponData` `CardData`.
- For in-game item instance: `CardItem` `CharacterItem`.
- For settings scripts inheriting Unity's ScriptableObject class: `AchievementSettings` `DailyLoginSettings`.
- For scripts spawning/initiating GameObjects: ` ObjectSpawners`, `EnemySpawners`.
- For editor-only scripts inheriting Unity's Editor class: `TutorialTaskEditor` `AchievementSettingsEditor`.

**Manager vs Controller**
> Difference between `Manager` and `Controller` is Manager should be singleton or static, and it controls a specific game logic that may involve multiple objects and assets, while Controller controls an object and may have multiple instances. For example, there are multiple EnemyController in the scene and each control one enemy.

**Data vs Item**
> Difference between `Data` and `Item` is that Item is an instance in-game, and in most cases Item contains a Data. For example CardData has all preset attributes of a card, while CardItem has its CardData plus attributes that vary for different players, such as card level. We will talk about more this in another data structure article. Sometimes Data and Database is not required and ScriptableObjects are used instead.


## General coding guidelines
- Avoid using public variables as much as possible. Use public methods instead, where you can control the input value and use Properties to expose your private variables.
- If possible, always initialize serialized variables and properties. Use `null` or `0` if needed.
- Consider using separate class for a Constant. Especially for a public one. 
- Use namespaces only in cases where your code is highly independent of other parts and serves as a separate library functionality. Unfortunately shortened form without the curly brackets and with semicolon at the end `namespace Memoria.Math;` is not supported by current Unity C# version (v9.0).
- Use `#region` only when truly necessary. Rather consider splitting the class into multiple classes if you feel like there is too much of the code.
- To raise events, use the question mark for the invocation: `HealthChanged?.Invoke`. It checks if any subscriber exists instead of throwing an error that you would need to handle in the subscriber class.
- Prefer descriptive identifiers before meaningless abbreviations. Example: Use `strengthChanged` instead of `strChgd`.
- Prefer identifiers that describe the semantics rather than just syntax. Example: `float damageMultiplier` instead of `float coefficient`
- Prefer (reasonably) longer and descriptive names. As an exception, it is OK to use short names sometimes, especially in mathematical expressions or when using well-known idioms. Examples that are all OK: `public class CharacterPhysicalDamage`, `for (int i = 0; i < 10; ++i)` or `float e = m * c * c`;
- Don't use abbreviations unless it's widely known. Some accepted abbreviations: `Ui`, `Sdk`, `Ai`. Abbreviations should start with upper case-letter and continue with lower case-letters.
- Remove unnecessary directives (Example: `using System.Linq`) in your code. It's okay to keep `using System;` even if not used.
- A class must be definitive, doing one thing. Example: Class `Spawner` should be split to `EnemySpawner`, `WeaponSpawner` and `AmmoSpawner`.
- Prefer composition over inheritance. Example: Use classes `Entity`, `Player`, `Health`, `Mana` instead of a base class `Entity` where would you have `protected int m_health` variable used by its inherited class `Player` -> the `Entity` class can exist, but `m_health` is not a variable that every `Entity` is going to have. Maybe indestructible concrete wall is `Entity` but can't be killed. So create separate class `Health` instead and add it to your damageable entities.

### Code formatting
- Use curly brackets `{}` on the new line (mainly for classes and methods). Never at the end of a line.
```csharp
public class ImbaClass : MonoBehaviour, IImbaInterface
{
    private void Jump()
    {
        ...
    }
}
```

### Variable naming
- Private and protected variables use prefix `m_xxXxx`, `m_itemCount` `m_titleText`.
- Private and protected static variables use prefix `s_xxXxx`, `s_maxBulletCount` `s_defaultPosition`.
- Public variables (including static) use `PascalCase`, `EnemyDescription` `SharedHealth`.
- Public and private constants use `ALL_CAPS`, `MAX_HP_COUNT` `BASE_DAMAGE`.
- Method variables use `camelCase`, `int result` `string name`.
- GameObject variables in the scene use suffix `xxxGO`, `optionButtonGO` `backgroundMaskGO`.
- GameObject prefabs use suffix `xxxPrefab`, `weaponSlotPrefab` `explosionPrefab`.
- Transform variables use suffix `xxxTF`, `weaponTF` `armTF`.
- Other Components use suffix by the name of the component `xxxComponent`, `eyesSpriteRenderer` `runAnimation` `attckAnimationClip` `victoryAudioClip`.
- Arrays are pluralized form of their values `xxxxs`, `slotPrefabs = new Prefabs[0]` `achievementIds = new int [0]`.
- Lists use suffix `xxxxList`,  `weaponTransformList = new List()`.
- Dictionary use suffix `xxxxDict`,  `achievementProgressDict = new Dictionary()`.

### Member Properties (getter/setter)
- Use Properties where a variable can only be set internally but must be accessed publicly: `public int Id { get; private set; }` `public Sprite IconSprite { get; private set; }`.
- If possible, use shortened form as `bool IsRewarded => ...;` instead.
  ```csharp
  // DO NOT USE THIS!!!
  public int Health
  {
      get
      {
          return m_health;
      }
      
      private set
      {
          m_health = value;
      }
  }
  
  // This is a prefered style
  public int Health => m_health;
  
  // If previous prefered style doesn't fit your needs, use this
  public int Health
  {
      get => return m_health;
      private set => m_health = value;
  }
  ```

### Attributes
- Try to keep all variable attributes in one line with the variable. First should be `[SerializeField]`, followed by your desired order of the other attributes, comma separated. Just keep it the order consistent in your classes, example: `[SerializeField, Range(0,10)]`.
- Multiple attributes, usually `[Tooltip("...")]` can be placed on multiple lines.
  ```csharp
   [FormerlySerializedAs("m_hp")]
   [Tooltip("This is a very interesting and important number in the game. It actually explains how health the player is and when is about to die.")]
   [SerializedField] 
   private int m_health;
  ```

### Functions / Methods
- In general, use `PascalCase`. Start with a verb and followed by a noun if needed `Reward()` `StartGame()` `MoveToCenter()`.
- Use `OnXxxxClick` for UI button clicks `OnStartClick()` `OnCancelClick()`.
- Use prefix `Debug` for debugging methods `DebugEnemyDetectionArea`.
- Always use `private` encapsulation for all methods, including Unity framework methods (they are without the `private` by default).

### Events and Subscriptions
- Callback events use `nounVerbed`, `public static UnityAction CharacterDied`.
- Callback subscriptions use `OnNounVerbed` `OnChestOpened()` `OnBuyWeaponConfirmed()`, therefore complete subscription could look like this:`Player.HealthChanged += OnPlayerHealthChanged`. 
- Subscribe to events in `OnEnable()` and unsubscribe in `OnDisable`, unless you have a special case like to subscribe to an event with a non-enabled script, than you'd need to use `Start()` and `OnDestroy()`.

### Comments
- Use first couple lines of the class to express what the class does using `//` per each line. You can also use it to write list of requirements or a pseudocode as what the class should do. You can keep these requirements/pseudocode once the class is finished - it can help other developers understand the class.
- Add `// Comments` at separate line instead of attaching it at the end of the line with code.
- Use `// TODO(name)` to see who created the TODO comment, `TODO(frosty)` `TODO(zeus)`.
- For comments, prefer the single-line `//` variant over the `/*` variant.
- Don't keep commented blocks of code. You can always find your previous code variants in Git!
- It's better to have meaningful and descriptive Class and Method names instead of writing the `/// <summary`. You can use the summary, but only if really necessary.
- Code should ideally be self-documenting, so avoid repeating things in comments that are obvious from the code itself.
- If you need to provide `//` comment to a variable, append it at the end of the variable line. `float m_jumpForce = 10.0f; // Represents how much player can jump.`
- Don't forget `.` at the end of the comment!
  ```csharp 
  // This is a description of what the ImbaClass does.
  // 
  // - Consider writing here a list of the requirements you want to follow in this class. It can help you when creating variables and methods.
  // - It's completely natural to keep the list of requirements once you are done with the class. It can help as a quick reference what to expect. Otherwise please provide a proper description.
      
  using System;
  using UnityEngine;
    
  public class ImbaClass : MonoBehaviour
  {
      public Action<int> HealthChanged;
  
      [SerializedField] private int m_health = 20; // Number of hit points player currently has.
        
      private void TakeDamage(int amount)
      {
          m_health -= amount;
          
          // Raises event with modified health after taking damage  
          HealthChanged?.Invoke(m_health);
      }
  }
  ```
  
### Enums
  - Use numbering whenever you find it reasonable. 
  - Always write enumerations on multiple lines.
    ```csharp
    public enum DebugCodes
    {
        Informative = 1,
        Warning,
        Error = 99
    }
    ```
    
### Code order
- Follow the standardized order of variables and methods as in the code example below.
- Try to keep variables grouped by their purpose.
  ```csharp 
  using System.Collections;
  using System.Collections.Generic;
  using UnityEngine;
  using Memoria.Math;
  
  namespace Memoria
  {
      public class ImbaClass : MonoBehaviour, IImbaInterface
      {
          // Actions, UnityActions, Delegates
          public Action GameStarted;
          public Func<int> MyFunction;
          public static UnityAction CharacterDied;
          public delegate void HealthChanged(int amount);
          
          // Events
          public event HealthChanged HealthChanged;
          
          // Public editor-assigned variables
          [SerializeField] private Image m_maskImage = null;
          
          // Private editor-assigned variables
          [SerializeField] private MyClass m_anotherComponent = null;
          
          // Public constants
          public const int MAX_HP_COUNT = 10;
          
          // Private constants
          private const int BASE_DAMAGE = 17;
              
          // Public Static variables
          public static string EnemyDescription = "A furious beast.";
          
          // Private Static variables
          private static int SharedHealth = 300;
              
          // Class public varaibles
          public int m_health;
          
          // Class private variables
          private bool m_isRewarded;
              
          // Public Properties
          public int IsRewarded => m_isRewarded;  
          
          public bool IsMuted
          {
              get { return m_isMuted; }
              set { m_isMuted = value; DoSmething(); }
          }
          
          // Private Properties
          private string Health => m_health; 
              
          // Unity methods in following order
          private Reset()
          private OnValidate()
          private Awake()
          private OnDestroy()
          private OnEnable()
          private OnDisable()
          private Start()
          private Update()
          
          // Public custom methods
          public Fire()

          // Private custom methods
          private Jump()
          
          #region UNITY_EDITOR
          // ContextMenu runs method from right-click at the script component
          [ContextMenu("DebugEnemyDetectionArea")]
          private void DebugEnemyDetectionArea() 

          // MenuItem runs method from Unity editor menu
          [MenuItem("Debug/Debug2")]
          private static void DebugResetPlayerPosition()
          #endregion
      }
  }
  ```

**Early exit**
- You can use early exist (also known as early return).
  ```csharp
  private void TakeDamage(Damage damage)
  {
      if (damage == null)
      {
          return;
      }
      
      m_health -= damage.Amount;
      HealthChanged?.Invoke(m_health);
  }
  ```

# Avoid this
- Don't use null-conditional or null-coalescing operators (i.e. ?. ?[] ?? ??=) with types derived from Unity.Object, for example with MonoBehavior or GameObject. 
  - The reason is that they don't do what you expect. If you write gameObject != null or simply (bool)gameObject, the overloaded comparison or cast operators call Unity.Object.CompareBaseObjects(Object lhs, Object rhs), whereas null-conditional and null-coalescing operators compare the reference itself to null.


# Unity methods execution order
https://docs.unity3d.com/6000.0/Documentation/Manual/execution-order.html

![[Pasted image 20241029001842.png]]
