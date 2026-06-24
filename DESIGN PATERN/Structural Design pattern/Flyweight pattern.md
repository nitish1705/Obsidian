
## Real-Life Coding Example

Imagine you're building a feature like Google Maps where you need to **visually represent trees** across the globe. Now, even though millions of trees are shown, most of them belong to only a few common types like “Oak”, “Pine”, or “Birch”. However, if we were to create a separate object for each individual tree — storing the same data repeatedly for tree type, color, and texture — it would lead to massive memory consumption.  
  
Let’s consider a scenario where we want to create **1 million trees**, all with the same name, color, and texture. A poor design would look like this:

#### Bad Design: Without Flyweight

Java

```java
import java.util.*;

// ================ Tree Class =================
class Tree {
    // Attributes that keep on changing 
    private int x;
    private int y;
    
    // Attributes that remain constant
    private String name;
    private String color;
    private String texture;
    
    public Tree(int x, int y, String name, String color, String texture) {
        this.x = x;
        this.y = y;
        this.name = name;
        this.color = color;
        this.texture = texture;
    }
    
    public void draw() {
        System.out.println("Drawing tree at (" + x + ", " + y + ") with type " + name);
    }
}

// ================ Forest Class =================
class Forest {

    private List<Tree> trees = new ArrayList<>();

    public void plantTree(int x, int y, String name, String color, String texture) {
        Tree tree = new Tree(x, y, name, color, texture);
        trees.add(tree);
    }

    public void draw() {
        for (Tree tree : trees) {
            tree.draw();
        }
    }
}

// =============== Client Code ==================
class Main {
    public static void main(String[] args) {
        Forest forest = new Forest();
        
        // Planting 1 million trees
        for(int i = 0; i < 1000000; i++) {
            forest.plantTree(i, i, "Oak", "Green", "Rough");
        }
        
        System.out.println("Planted 1 million trees.");
    }
}
```

###### Understanding the Issues

Although the above codes works absolutely fine but there are a few problems associated with it:

- **Redundant memory usage:** Same tree data duplicated a million times.
- **Inefficient:** Slower rendering, higher GC overhead.

  
The previous implementation created **a new Tree object for each of the 1 million trees**, even when most of them had identical properties like name, color, and texture. This led to unnecessary duplication of memory for the shared attributes.   
  
To solve this, we use the **Flyweight Design Pattern** — a structural pattern focused on minimizing memory usage by **sharing as much data as possible between similar objects**.   
  

#### Good Design: Using Flyweight Pattern

Java

```java
import java.util.*;

// ============= TreeType Class ================
class TreeType {
    // Properties that are common among all trees of this type
    private String name;
    private String color;
    private String texture;

    public TreeType(String name, String color, String texture) {
        this.name = name;
        this.color = color;
        this.texture = texture;
    }

    public void draw(int x, int y) {
        System.out.println("Drawing " + name + " tree at (" + x + ", " + y + ")");
    }
}


// ================ Tree Class =================
class Tree {
    // Attributes that keep on changing 
    private int x;
    private int y;
    
    // Attributes that remain constant
    private TreeType treeType;
    
    public Tree(int x, int y, TreeType treeType) {
        this.x = x;
        this.y = y;
        this.treeType = treeType;
    }
    
    public void draw() {
        treeType.draw(x, y);
    }
}


// ============ TreeFactory Class ==============
class TreeFactory {

    static Map<String, TreeType> treeTypeMap = new HashMap<>();

    public static TreeType getTreeType(String name, String color, String texture) {
        String key = name + " - " + color + " - " + texture;

        if (!treeTypeMap.containsKey(key)) {
            treeTypeMap.put(key, new TreeType(name, color, texture));
        }
        return treeTypeMap.get(key);
    }
}


// ================ Forest Class =================
class Forest {
    private List<Tree> trees = new ArrayList<>();

    public void plantTree(int x, int y, String name, String color, String texture) {
        Tree tree = new Tree(x, y, TreeFactory.getTreeType(name, color, texture));
        trees.add(tree);
    }

    public void draw() {
        for (Tree tree : trees) {
            tree.draw();
        }
    }
}


// =============== Client Code ==================
class Main {
    public static void main(String[] args) {
        Forest forest = new Forest();
        
        // Planting 1 million trees
        for(int i = 0; i < 1000000; i++) {
            forest.plantTree(i, i, "Oak", "Green", "Rough");
        }
        
        System.out.println("Planted 1 million trees.");
    }
}
```

###### How Flyweight Pattern Solves the Issue

Let’s break it down:

- **`TreeType` Class:** This acts as the **flyweight** object. It stores data **common** to all trees of a given type—like name, color, and texture. Instead of duplicating this data, we create **only one instance per unique combination**.
- **`Tree` Class:** This now only stores: 
    - **Intrinsic data:** x, y (unique to each tree)
    - **Reference to shared data:** A TreeType instance
- **`TreeFactory` Class:** This is the **central factory** that ensures **TreeType** instances are reused:
- **Memory Efficiency:** Even with **1 million trees**, if they all share the same TreeType ("Oak", "Green", "Rough"), **only one TreeType object is created** and shared across all trees, reducing memory usage **dramatically**.

  

---

## When to Use Flyweight Pattern?

The flyweight pattern can be used when: - When you need to create a large number of similar objects.
- When memory and performance optimization is crucial.
- When the object's **intrinsic properties** could be shared independently of its **extrinsic properties**.