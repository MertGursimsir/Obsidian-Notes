- `if task.instruction == InstructionTypes.INSTALL or task.instruction == InstructionTypes.UPGRADE`
	- Bunun yerine bu kullanılabilir:
		- `if task.instruction in [InstructionTypes.INSTALL,  InstructionTypes.UPGRADE]`

- Her eleman yeni satırda:
	- `print(*job[command], sep='\n')`

