# extender代码

```
func filter(args schedulerapi.ExtenderArgs) *schedulerapi.ExtenderFilterResult { 
var filteredNodes []v1.Node 
failedNodes := make(schedulerapi.FailedNodesMap) 
pod := args.Pod 
for _, node := range args.Nodes.Items { 
fits, failReasons, _ := podFitsOnNode(pod, node) 
if fits { 
filteredNodes = append(filteredNodes, node) 
} else { 
failedNodes[node.Name] = strings.Join(failReasons, ",") 
} 
}
result := schedulerapi.ExtenderFilterResult{ 
Nodes: &v1.NodeList{ 
Items: filteredNodes, 
},
FailedNodes: failedNodes, 
Error: "", 
}
return &result 
}
func prioritize(args schedulerapi.ExtenderArgs) *schedulerapi.HostPriorityList { 
pod := args.Pod 
nodes := args.Nodes.Items 
hostPriorityList := make(schedulerapi.HostPriorityList, len(nodes)) 
for i, node := range nodes { 
term = (term + 1) % (schedulerapi.MaxPriority + 1) 
score := term 
log.Printf(luckyPrioMsg, pod.Name, pod.Namespace, score) 
hostPriorityList[i] = schedulerapi.HostPriority{ 
Host: node.Name, 
Score: score, 
} 
}
return &hostPriorityList 
}
```java


extender运行逻辑：<br>
首先extender和scheduler建立http连接，scheduler传送json，extender接收后读取node信息，分配任务，得到extenderFilterResult和hostPriorityList后，再打包为json发送给scheduler进行调度
