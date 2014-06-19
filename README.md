PTO Template Designer
------------------------------------------------------------
# 功能说明
实现动态可配的PTO计算模型， 用户可自行定义计算逻辑及前台样式。

逻辑对象定义
------------------------------------------------------------
# Node Field
定义在policy node中，作为PTO Policy的输入参数，条件限制，计算参数等。
* - name, 作为field的唯一标识
* - value, field值  
			
### policy Node
		PTO Template组成单位，最小的逻辑单位。
### policy Template
		PTO Policy的模板，由policy node组成，可以被PTO Policy使用。
		# 属性及方法
			*Id 唯一标识 guid
			*Name 模板名称
			*Pattern 存放*node expression
			*Remark 描述
			*Nodes 存放template中包含的所有node对象, 由Pattern转换而来
			#Invoke 执行模板所有node, 返回执行结果, 当Nodes中不包含节点会抛出异常


### PTO Policy
		PTO计算策略，用于计算员工年假。对于完整的PTO policy, 要求用户选择对应policy template，并设置policy template中node field的		值。
### Policy Scope Context
		Policy Node的逻辑计算上下文, 用于存放逻辑计算结果。
### PTO Employee Context
		系统内置用户信息上下文，用于存放用户相关信息，及内置PTO相关属性值。
### ResultContext
		Template及Node执行结果，可以包含结果数据及流程控制。

计算逻辑说明
------------------------------------------------------------

Policy Template表达式 ( node expression )
------------------------------------------------------------
公开的服务接口
------------------------------------------------------------
### Web api
		api/Transnode	负责提供IPolicyNode与Node expression间的数据转换服务
		POST 主要转换方法，接受json数据，返回json数据。
			包含'node'时，返回该node表达式
			包含'nodes'时，返回这些nodes表达式
			包含'content'时，返回表达式对应的node对象
			包含'contents'时，返回表达式对应的node集合
### Template api.
### PTOContext api.

template node design
------------------------------------------------------------
### template
### code
		用户需要添加脚本代码以定义node的计算逻辑，计算逻辑用c#脚本代码实现。
		脚本以下需要遵循下列格式

 < public void Invoke(ActionContext context, ActionResult result)
 < {
 < < //input calc logic code
 < }
		该格式定义是接口ICodeInvoker的一个片段，以下为ICodeInvoker的定义
		public interface ICodeInvoker
		{
			dynamic Scope { get; set; }
			void Invoke(ActionContext context, ActionResult result);
		}



Task List
------------------------------------------------------------
# 06132014 
		task 0. 修改原template Tree node结构为顺序结构
			subtask 0.0 修改后台计算逻辑
				* 修改PolicyTemplate.cs 添加 invoke方法
				* 修改PolicyNode.cs
			subtask 0.1 修改Controller
				* 修改TransnodeController
				* 修改PolicyTranslator
			subtask 0.2 修改Gui
			    * 修改NodeModel.js
# 06192014
		task 1. 修改用户代码后台执行方式，添加ICodeInvoker.

Change Log
------------------------------------------------------------
# 06132014
		api/Transnode 添加集合转换方法		

				
		
		
