# Dcrew.Spatial
 A set of highly-optimized, flexible and powerful 2D spatial partitions for [MonoGame](https://github.com/MonoGame/MonoGame)

## Build
### [NuGet](https://www.nuget.org/packages/Dcrew.Spatial) [![NuGet ver](https://img.shields.io/nuget/v/Dcrew.Spatial)](https://www.nuget.org/packages/Dcrew.Spatial) [![NuGet downloads](https://img.shields.io/nuget/dt/Dcrew.Spatial)](https://www.nuget.org/packages/Dcrew.Spatial)

## How to use
1. An item must inherit IBounds (interface)
```cs
class Item : IBounds {
 public Vector2 XY { // Position
  get => _bounds.XY;
  set => _bounds.XY = value;
 }
 public float X { // X Position
  get => _bounds.XY.X;
  set => _bounds.XY.X = value;
 }
 public float Y { // Y Position
  get => _bounds.XY.Y;
  set => _bounds.XY.Y = value;
 }
 public Vector2 Size { // Size of bounds
  get => _bounds.Size;
  set => _bounds.Size = value;
 }
 public float Rotation { // Rotation (in radians)
  get => _bounds.Rotation;
  set => _bounds.Rotation = value;
 }
 public Vector2 Origin { // Origin
  get => _bounds.Origin;
  set => _bounds.Origin = value;
 }
 
 RotRect IBounds.Bounds => _bounds; // want to expose .Bounds? make this public and remove 'IBounds.'
 
 RotRect _bounds = new RotRect { Size = new Vector2(4, 7) };
}
```

2. Make a Quadtree variable
```cs
Quadtree<Item> tree = new Quadtree<Item>();
```

3. Add an item(s)
```cs
var itemA = new Item {
 XY = new Vector2(0, 0),
 Size = new Vector2(10, 6),
 Origin = new Vector2(5, 3), // center
 Angle = .5f
};
tree.Add(itemA);
var itemB = new Item {
 XY = new Vector2(30, 20),
 Size = new Vector2(15, 10),
 Origin = new Vector2(7.5f, 5), // center
 Angle = 1.2f
};
tree.Add(itemB);
```

4. Query an area(s)
```cs
using (var query = tree.QueryPoint(xy: new Point(3, 4))) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.QueryPoint(xy: new Vector2(32.5f, 25))) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.QueryRect(area: new Rectangle(x: 7, y: 2, width: 32, height: 27), rotation: 0, origin: Vector2.Zero)) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.QueryRect(new RotRect(x: 7, y: 2, width: 32, height: 27, rotation: 0, origin: Vector2.Zero))) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.QueryRadius(xy: new Point(3, 4), radius: 10)) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.Linecast(start: new Vector2(3, 4), end: new Vector2(8, 12), thickness: 3)) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.Raycast(start: new Vector2(3, 4), direction: new Vector2(.5f, .75f), thickness: 3)) {
 foreach (var item in query) {
  // ...
 }
}
using (var query = tree.Raycast(start: new Vector2(3, 4), rotation: MathF.PI, thickness: 3)) {
 foreach (var item in query) {
  // ...
 }
}
```

5. When an item moves, update it!
```cs
Item itemA; // has its xy, angle, size, or origin changed?
tree.Update(itemA); // call this if so, it's incredibly optimized so don't worry!
```

6. Did an entity despawn? Remove it!
```cs
Item itemA;
tree.Remove(itemA);
```

7. Don't use 'base.Update()' in your 'Game1.Update()'? Call this at the start or end of your game/scene update!
```cs
tree.Update();
```
