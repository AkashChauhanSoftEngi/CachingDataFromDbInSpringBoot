# CachingDataWithSpringBoot

* Need to add @EnableCaching with main method class
* Ehcache is being used at service implementation level/layer
* @Cacheable(value="key"): to store return type of this method as key in the Cache 
* @CacheEvict(value = "key",allEntries = true): Clear it from cache
* When server start the cache will not have any value in the JVM{HashMap/Hashtable/RAM}
* Reference: https://java2practice.com/2013/03/23/spring-cacheable-and-cacheevict-explained-in-simple-terms/

## @Cacheable(---)
* To store this value/ds in cache
* If value exist already, then will not execute that method. If not exist then, execute that method.
* Whenever this value is updated, that update operation need to delete it from the cache after updating data in db 
```java
	@Cacheable(value="messagecache", key="#id", condition="id < 10")
	public String getMessage(int id){
	return "hello"+id;
}
```
* @Cacheable has 3 attributes
* Value : is the cache name and it is mandatory, in example it is “messagecache”
* Key: based on this data will be cached and it is optional
* Condition:  based on the condition data will be cached. In example if the id < 10 then only data will be cached otherwise won’t. it is optional

## @CacheEvict(---)
* To delete this value “employees” from Cache
* @CacheEvict has 5 attributes:
* Value, 2 Key, 3 condition are similar to @Cacheable, apart from these 3 we have another 2 attributes
* allEntries : is a Boolean type and delete entire cache
* beforeInvocation: is Boolean type and will delete the cache before the method execution
```java
	@CacheEvict(value = “employees”, beforeInvocation = true)
	public void saveEmployee(Employee e){}
```

## Clear Cache using CacheManager
* In the given code, name represent the individual values in the cache like “employees”
```java
	@Autowired
    private CacheManager cacheManager;      // autowire cache manager
	
	// clear all cache using cache manager
    @RequestMapping(value = "clearCache")
    public void clearCache(){
        for(String name:cacheManager.getCacheNames()){
            cacheManager.getCache(name).clear();
        }
    }
```
