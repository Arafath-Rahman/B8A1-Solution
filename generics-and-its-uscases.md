## Generics

<div style="text-align: justify;">Generics is not a new concept that introduced in Typesccript. The concept of generic programming pioneered in early 70’s programming languages like Meta Language (ML), Cluster (CLU), Fortran under the concept of parametric polymorphism and parameterized types. Modern programming lanuages like C++, C#, and Java uses generics too.
<br>
<br>
Generics allow us to make our code reusable with various type of data instead of a fixed single one while maintaing type safety. It works as a placeholder for the types. If we write a function with a fixed data type we are limiting our function with that data type only. Here,

```tsx
function hello(data: string): string {
  return data;
}

hello("I take String only");
```

this `hello` function can only take and return `string` type data. If we need to take some other type of data in future, this function can’t do that. Now,

```tsx
function helloWithGenerics<T>(data: T): T {
  return data;
}

helloWithGenerics("I can take String");
helloWithGenerics(123);
helloWithGenerics(true);
helloWithGenerics(undefined);
```

in the `helloWithGenerics` function, we can provide any data types and it’ll work. The code won’t break. In typescript we can use the `any` data type here as well, but the downside is that we will loose the data type information when we use `any`.

```tsx
function helloWithAny(data: any): any {
  return data;
}

const myData = helloWithAny("i am a string");
typeof myData; //any
```

Here, in the `helloWithAny` function, we are passing a string, but when we’re receiving the `data` into `myData` variable, we are loosing the information that ‘data’ was a string and now its ‘any’ type.

Alongside with functions, generics can be used with interfaces and classes as well. Here,

```tsx
interface Bird {
  id: number;
  name: string;
  canFly: boolean;
}

interface IZoo<T> {
  sanctuary: T[];
}

class CZoo<T> implements IZoo<T> {
  sanctuary: T[] = [];

  constructor(initialAnimals?: T[]) {
    if (initialAnimals) {
      this.sanctuary = initialAnimals;
    }
  }

  addAnimal(animal: T): void {
    this.sanctuary.push(animal);
  }

  getAnimals(): T[] {
    return this.sanctuary;
  }
}

const initialBirds: Bird[] = [
  { id: 1, name: "Sparrow", canFly: true },
  { id: 2, name: "Parrot", canFly: true },
];

const birdZoo = new CZoo<Bird>(initialBirds);

birdZoo.addAnimal({ id: 3, name: "Penguin", canFly: false });

console.log(birdZoo.getAnimals());
```

this example can show the vastness of using generics. If we have been asked to create separate zoo for other animals like Cats, Dogs, and Elephants, we can just create the animal types and then by reusing the `IZoo` interface, and `CZoo` class our separate zoo will be ready.

Generics has some advanced safety rules like constraints (`extends`, `keyof`), `defaults` to make our system extra safe. For example:

```tsx
interface DataBox<T = string> {
  content: T;
}

const textDataBox: DataBox = { content: "Hello World" };
const numberDataBox: StorageBox<number> = { content: 123 };
```

Here, we have used Default types (string) in `DataBox`, but when we’ve created the `numberDataBox`, we are overriding the content type from `string` to `number`.

```tsx
const phone = { brand: "Apple", ram: 16 };

function getPhoneSpec<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

getPhoneSpec(phone, "ram"); //ok
getPhoneSpec(phone, "cpu"); //error
```

Here, `T` is the placeholder for an object, when the function receives an object in place of `T`, then `K` (key) extends the key of that object by `keyof`, and extracts all propety keys of that object as options. If we now try to use any other keys other than `brand | ram`, it’ll give error. Thats how the constraints provides gurdrails and make typescript more safe.</div>

### How generics help working with asynchronous `Promise<T>` responses

<div style="text-align: justify;">Whenever developers work with APis, one of the concerned factor is how the response data will arrive. By using generics we can make sure that the response data we’ll get will be according the type we’ll set.

```tsx
interface Dog {
  id: number;
  species: string;
}
interface ApiResponse<T> {
  data: T;
  message: string;
  status: string;
}

async function fetchRandomDog(): Promise<ApiResponse<Dog>> {
  const response = await fetch("https://dog.ceo");
  const data = (await response.json()) as ApiResponse<Dog>;
  return data;
}

async function showDogPicture() {
  const dogData = await fetchRandomDog();
  console.log(dogData.data.id); // prints id
  dogData.bark(); // gives error
}
```

Here, by providing `Dog` into `ApiResponse<T>`, we are ensuring that the data we’ll get from the `Promise<>` will definitely have `Dog` type data, otherwise we would have set `any` type and hope that it will return the `Dog` type data. And now if we want to receive a `Cat` type data, we can use the same method, only we have to create a `Cat` interface.

Generics are very powerful tool that enables us to make our code reusable and maintainable. When we are working with multiple data types it helps us to handle all those type while keeping our code type safe. We should highly use generics and it's features in our code more frequently.</div>
