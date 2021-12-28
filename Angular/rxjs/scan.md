# How to update objects

## Old way of updating an object
```
import { Observable, ReplayuSubject } from 'rxjs;

export interface Car {
    id: number;
    make: string;
    color: string;
}

export class CarService {
    private carBhvSubj = ReplaySubject<Car> = new ReplaySubject<Car>(1);
    car$: Observable<Car> = this.carBhvSubj.asObservable();

    updateCar(carPart: Car): void {
        this.carBhvSubj.next(carPart);
    }
}

const carService = new CarService();
carService.car$.subscribe(console.log);

carService.updateCar({ make: 'BMW', color: 'White', id: 1});

```

### What are we trying to solve?
1. We dont always want to update all the fields at the same time.
2. Only some properties have to be set to defaults.
3. Passing the whole object when we want to modify only a part, is not necessary.
4. We want to get rid of an additional Object Reference. Mission is Single Responsibility Principle.

### SOLUTION:
**scan operator and Partial utility type**

```
scan((oldValue: Partial<Car>, newValue: Partial<Car>)) => {
    return { ...oldValue, ...newValue };
}
```

* scan operator might be used to manage the state.
* Applies an accumulator to each value from the source after an initial value is established.

### Partial
* When we have an interface or type with required fields, a Partial built-in type could help to create a new type from an already existing object with partial implementation.
https://obaranovskyi.medium.com/typescript-understand-built-in-utility-types-5aa9ea44fe45

```
import { Observable, ReplayuSubject } from 'rxjs;

export interface Car {
    id: number;
    make: string;
    color: string;
}

export class CarService {
    private carBhvSubj = new BehaviorSubject<Partial<Car>>({ color: 'White' });
    car$ = this.carBhvSubj.asObservable()
        .pipe(
            scan((oldValue: Partial<Car>, newValue: Partial<Car>) => {
                return { ...oldValue, ...newValue };
            });
        )

    updateCar(carPart: Car): void {
        this.carBhvSubj.next(carPart);
    }
}

const carService = new CarService();
carService.car$.subscribe(console.log);

carService.updateCar({ make: 'BMW'});

```

### Make it reusable:
```
import { Observable, ReplayuSubject } from 'rxjs;

export interface Car {
    id: number;
    make: string;
    color: string;
}

export function assign<T>(): OperatorFunction<Partial<T>, Partial<T>> {
    return scan((oldValue: Partial<T>, newValue: Partial<T>) => {
        return { ...oldValue, ...newValue };
    });
}

export class CarService {
    private carBhvSubj = new BehaviorSubject<Partial<Car>>({ color: 'White' });
    car$ = this.carBhvSubj.asObservable().pipe(assign());

    updateCar(carPart: Partial<Car>): void {
        this.carBhvSubj.next(carPart);
    }
}

const carService = new CarService();
carService.car$.subscribe(console.log);

carService.updateCar({ make: 'BMW'});

```

### Adding validation
**Most likely this will be used on a form with a submit button, so lets add validation**
```
import { Observable, ReplayuSubject } from 'rxjs;

export interface Car {
    id: number;
    make: string;
    color: string;
}

export function assign<T>(): OperatorFunction<Partial<T>, Partial<T>> {
    return scan((oldValue: Partial<T>, newValue: Partial<T>) => {
        return { ...oldValue, ...newValue };
    });
}

export class CarService {
    private carBhvSubj = new BehaviorSubject<Partial<Car>>({ color: 'White' });
    car$ = this.carBhvSubj.asObservable().pipe(assign());

    isCarValid$ = this.car$
        .pipe(
            map((car: Partial<Car>) => {
                return Boolean(car.color && car.make);
            })
        )

    updateCar(carPart: Partial<Car>): void {
        this.carBhvSubj.next(carPart);
    }
}

const carService = new CarService();
carService.car$.subscribe(console.log);

carService.updateCar({ make: 'BMW'});
```

**We could also extract the Partial<Car> into a new type:**
```
type PartialCar = Partial<Car>;
```


