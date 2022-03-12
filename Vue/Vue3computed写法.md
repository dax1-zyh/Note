```javascript
import {computed} from 'vue'

setup(){
	const person = reactive({
		firstName:''，
		lastName:''
	})
	
	// 计算属性-简写（只读不可改）
	person.fullName = computed(()=>{
		return person.firstName + '-' person.lastName
	})
	
	// 计算属性-完整写法（可读可修改）
	person.fullName = computed({
		get(){
			return ...
		}
		set(value){
			doSomething
		}
	})
}
```