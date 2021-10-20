# Exercises 009 Communicating Between GameObjects

## Preparation

Read the slides on [003.6.2 - Console Intermediate: Generics](https://github.com/marczaku/csharp-intermediate/blob/main/slides/003.6-console-intermediate-2-generics.md) and [009 - Communicating Between GameObjects](../slides/009-communicating-between-gameobjects.md)

## Zoo
It's time again to create a small Console-Project! Create it within `projects` and name it `ZooMZ`, where `MZ` needs to replaced with your Initials.

### Preparing the Animals

Add the following classes:
- `Animal`
- `Mammal` which inherits from `Animal`
- `Bear` which inherits from `Mammal`
- `Donkey` which inherits from `Mammal`
- `Lion` which inherits from `Mammal`
- `Fish` which inherits from `Animal`
- `Salmon` which inherits from `Fish`
- `Clownfish` which inherits from `Fish`
- `Student`

### Creating a Zoo

Now, add a class named `Zoo` with one Generic Type Parameter with a Constraint or no Constraint of your choice.

The Idea of this class is, that we'll be able open up different Zoos for different kinds of Animals.\
We want to have a `Fish`-Zoo, which can only contain Fishes, like `Salmon` and `Clownfish`.\
And a `Mammal`-Zoo, which can only contain Mammals, like `Bear` and `Donkey`.\
And a `Donkey`-Zoo, which can only contain Donkeys.\
We do not want to have Finishes in our `Mammal`-Zoo.\
We do not want to be able to create a `Student`-Zoo.\
We want to create one Generic-Class, which can cover all of these cases.

For that purpose, we want to have a `public` Method named `AddAnimal` that takes an Animal als an Argument.\
But only the kinds of Animals that are allowed in this Zoo...
- You can store the Animals in an Array.
  - Add an Array of the correct Type as a Field to your Zoo.
- You will probably need to Resize your Array many times using `Array.Resize(ref array);`

Also, we want to have one `public` Method named `HasAnimal` that can find out for us, whether the `Zoo` currently has a certain kind of Animal in the Zoo.\
It has one Generic Type Parameter, which lets us pass the Type of the Animal that we want to look for in the Zoo.\
And return a `bool` that is `true`, if that kind of Animal exists in the Zoo, and `false`, if not.\
This function also should only accept requests that actually make sense...\
We don't want someone asking, whether there's currently a `Lion` in the `Fish`-Zoo...

### Testing the Zoo

A few Test-Cases for you:

#### Correct animal can be added:

```cs
{
  Zoo<Fish> fishZoo = new Zoo<Fish>();
  fishZoo.AddAnimal(new Fish()); // OKAY
  fishZoo.AddAnimal(new Clownfish()); // OKAY
}
```

```cs
{
  Zoo<Animal> animalZoo = new Zoo<Animal>();
  animalZoo.AddAnimal(new Fish()); // OKAY
  animalZoo.AddAnimal(new Clownfish()); // OKAY
  animalZoo.AddAnimal(new Lion()); // OKAY
  animalZoo.AddAnimal(new Donkey()); // OKAY
}
```

```cs
{
  Zoo<Lion> lionZoo = new Zoo<Lion>();
  animalZoo.AddAnimal(new Lion()); // OKAY
  animalZoo.AddAnimal(new Lion()); // OKAY
  animalZoo.AddAnimal(new Lion()); // OKAY
}
```

#### We can not create a Student-Zoo:

```cs
{
  Zoo<Student> studentZoo = new StudentZoo(); // ERROR!
}
```

#### We cannot add wrong animals to the Zoos:

```cs
{
  Zoo<Fish> fishZoo = new Zoo<Fish>();
  fishZoo.AddAnimal(new Lion()); // ERROR!
}
```
]

```cs
{
  Zoo<Animal> animalZoo = new Zoo<Animal>();
  animalZoo.AddAnimal(new Student()); // ERROR!
}
```

```cs
{
  Zoo<Salmon> salmonZoo = new Zoo<Salmon>();
  salmonZoo.AddAnimal(new Fish()); // ERROR!
}
```

#### We cannot call the HasAnimal Method with senseless questions:

```cs
{
  Zoo<Salmon> salmonZoo = new Zoo<Salmon>();
  salmonZoo.HasAnimal<Lion>(); // ERROR!
}
```

#### It returns false, if the Animal is not contained:

```cs
{
  Zoo<Fish> fishZoo = new Zoo<Fish>();
  fishZoo.AddAnimal<Salmon>();
  fishZoo.AddAnimal<Salmon>();
  Console.WriteLine("This should be False: "+fishZoo.HasAnimal<Clownfish>());
}
```

#### It returns true, if the Animal is contained:

```cs
{
  Zoo<Fish> fishZoo = new Zoo<Fish>();
  fishZoo.AddAnimal<Salmon>();
  fishZoo.AddAnimal<Clownfish>();
  fishZoo.AddAnimal<Salmon>();
  Console.WriteLine("This should be True: "+fishZoo.HasAnimal<Clownfish>());
}
```

```cs
{
  Zoo<Animal> animalZoo = new Zoo<Fish>();
  animalZoo.AddAnimal<Salmon>();
  animalZoo.AddAnimal<Lion>();
  animalZoo.AddAnimal<Donkey>();
  Console.WriteLine("This should be True: "+fishZoo.HasAnimal<Fish>());
}
```

## Small Theft Auto Code Improvements

Go through your Small Theft Auto Project and see, if you can replace a few of those `public` Component Reference Fields.

e.g.: Does the `Player` need a Reference to the `Car`? Or vice versa? Probably not! I'm sure that you can get these References in a smarter way using `GetComponent`, and `FindObjectOfType` and a bunch of the others.

The less we need to set up manually, the better, because the less we can forget to set up!

One example:\
Have the Player have a `Driver` Script.
- In `Update`, you check if `F` is Pressed Down.
- If so, you use `FindObjectsOfType<T>` to search for all `Vehicle`-Scripts in your Surroundings.
- BASIC: You check for the first one (`[0]`) and whether it's close enough, and hope that it's always just one car.
- BONUS: You find the one of all `Vehicle`, that's the closest to you and check, whether that one's close enough.
- If it is, you call `Enter` on that `Vehicle`, passing the `Driver` into the Method.

The `Enter`-Method:
- Stores the `Driver` to a `private` field named `driver`.
- Disables the `Driver`'s GameObject.
- Enables the `VehicleMovement`-Component on its own GameObject using `GetComponent<T>`

The `Vehicle`:
- In `Update` aalso checks, if `F` is Pressed Down.
- If so, you check, whether the `driver` field is null or not.
- If not, then you call `Leave`

The `Leave`-Method:
- Moves the `driver`'s GameObject to the Vehicle's position.
- Enables the `driver`'s GameObject.
- Sets the `driver` field to `null` (to make it clear, that there is no more driver in this car)
- Disables the `VehicleMovement`-Component on its own GameObject using `GetComponent<T>`
