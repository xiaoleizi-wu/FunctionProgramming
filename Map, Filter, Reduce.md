## Map, Filter, Reduce

### 实例代码
```
struct City {
    let name: String
    let population: Int
}
```
```
class Test_02Controller: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        func incrementArray(xs: [Int]) -> [Int] {
            var result: [Int] = []
            for x in xs {
                result.append(x + 1)
            }
            return result
        }
        
        
        func doubleArray1(xs: [Int]) -> [Int] {
            var result: [Int] = []
            for x in xs {
                result.append(x * 2)
            }
            return result;
        }
        
        // 大量的重复代码
        
        // computInrArray: 可以理解为一个函数族
        func computIntArray<Element ,T>(xs: [Element], transform: (Element) -> T) -> [T] {
            var result: [T] = []
            for x in xs {
                result.append(transform(x))
            }
            return result
        }
    
        func genericComputArray2<T>(xs: [Int], transfrom: (Int) -> T) -> [T]{
            return computIntArray(xs: xs, transform: transfrom)
        }
     
    
        func doubleArray2(xs: [Int]) -> [Int] {
            return computIntArray(xs: xs, transform: { (x) -> Int in
                return x * 2
            })
        }
        
        
        // map
        func genericComputeArray<T>(xs: [Int], traansform: (Int) -> T) -> [T] {
            return xs.map(transform: traansform)
        }
        
        
        
        // filter
        func getSwiftFiles2(files: [String]) -> [String] {
            return files.filter(includeElement: { (file) -> Bool in
                return file.hasPrefix(".swift")
            })
        }
        
        // Reduce
        
        func sum(xs: [Int]) -> Int {
            var result: Int = 0
            for x in xs {
                result += x
            }
            return result
        }
        
        func product(xs: [Int]) -> Int {
            var result: Int = 1
            for x in xs {
                result = x * result
            }
            return result
        }
        
        func concatenate(xs: [String]) -> String {
            var result: String = ""
            for x in xs {
                result += x
            }
            return result
        }
        
        func sumUsingReduce(xs: [Int]) -> Int {
            return xs.reduce(initial: 0, combine: { (result, x) -> Int in
                return result + x
            })
        }
        
        func productUsingReduce(xs: [Int]) -> Int {
            return xs.reduce(initial: 1, combine: *)
        }
        
        func contactUsingReduce(xs: [String]) -> String {
            return xs.reduce(initial: "", combine: +)
        }
        
        
    }
    
    
    func example() {
     
        
        let paris = City.init(name: "Paris", population: 2241)
        let madrid = City.init(name: "Madrid", population: 3165)
        let amsterdam = City.init(name: "Amsterdam", population: 827)
        let berlin = City.init(name: "Berlin", population: 3562)
        
        let cities = [paris, madrid, amsterdam, berlin]
        
        cities.filter(includeElement: {$0.population > 1000})
            .map(transform: {$0.cityByScalingPopulation()})
            .reduce(initial: "City Population") { (result, c) -> String  in
                return result + "\n"  + "\(c.name): \(c.population)"
        }
        
        
    } 
    
}

```
```

extension City {
    func cityByScalingPopulation() -> City {
        return City.init(name: name, population: population * 1000)
    }
}
```
```
extension Array {
    func map<T>(transform: (Element) ->T) ->[T] {
        var result: [T] = []
        for x in self {
            result.append(transform(x))
        }
        return result
    }
}
```
```
extension Array {
    func filter(includeElement:(Element) -> Bool) -> [Element]{
        var result: [Element] = []
        for x in self where includeElement(x) {
            result.append(x)
        }
        return result
    }
}
```
```
extension Array {
    func reduce<T>(initial: T, combine: (T, Element) -> T) ->T {
        var result = initial
        for x in self {
            result = combine(result, x)
        }
        return result
    }
}
```