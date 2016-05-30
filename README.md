# eventdispatcher
eventdispatcher written by golang

# Usage

	package main
	
	import (
		"log"
	
		"github.com/sryanyuan/eventdispatcher"
	)
	
	const (
		kEvent_SayHello = iota
	)
	
	type SayHelloArg struct {
		times int
	}
	
	func sayHello1(data interface{}) {
		arg, ok := data.(*SayHelloArg)
		if !ok {
			return
		}
	
		for i := 0; i < arg.times; i++ {
			log.Println("hello1\n")
		}
	}
	
	func sayHello2(data interface{}) {
		arg, ok := data.(*SayHelloArg)
		if !ok {
			return
		}
	
		for i := 0; i < arg.times; i++ {
			log.Println("hello2\n")
		}
	}
	
	func main() {
		dip := eventdispatcher.NewEventDispatcher()
		handle1 := dip.AddListener(kEvent_SayHello, sayHello1)
		handle2 := dip.AddListener(kEvent_SayHello, sayHello2)
		arg := SayHelloArg{
			times: 3,
		}
		dip.Dispatch(kEvent_SayHello, &arg)
	
		//	remove listener
		dip.RemoveListener(handle1)
		dip.RemoveListener(handle2)
		dip.Dispatch(kEvent_SayHello, &arg)
	}
