# **pywombo**
[![Telegram channel](https://img.shields.io/endpoint?url=https://runkit.io/damiankrawczyk/telegram-badge/branches/master?url=https://t.me/bots_forge)](https://t.me/bots_forge)

**A library for simple usage https://dream.ai (neural network of image generation) from python code.** 
[![N|Solid](https://upload.wikimedia.org/wikipedia/commons/d/d7/WomboLogo.svg)](https://dream.ai/create)


```bash
pip install pywombo
```
**Depencies**: `pydantic, httpx, httpx-socks, proxystr`
**Optional**: `pillow`

## **Simple usage**
```python
from wombo import Dream

dream = Dream()
task = dream.generate('a dragon in the sky')  # by default a random style used

print(task.url)
# or
task.image.save(folder='images')
# or
task.image.show()  # this needs pillow installed
```
- **async usage**
```python
import asyncio
from wombo import AsyncDream

dream = AsyncDream()

async def get_image(prompt):
    task = await dream.generate(prompt)
    return task.image

image = asyncio.run(get_image('a dragon in the sky'))

url = image.url
image.save()  # by default folder='images'
PIL_Image = image.image  # this needs pillow installed
image.show()  # this needs pillow installed
```

### Styles
```python
top_styles = dream.styles.top
free_styles = dream.styles.free
all_styles = dream.styles.all
random_style_from_top = dream.styles.random()

for style in top_styles:
    print(f"{style.id:<5}{style.name}")

task = dream.generate('a dragon in the sky', style=115)
# or
task = dream.generate('a dragon in the sky', style=top_styles[10])
```

### Proxy
```python
from wombo import Dream

dream = Dream('login:password@ip:port')
dream = Dream('socks5://login:password@ip:port')
```
`Dream` takes proxy in any popular format because it uses [`proxystr`](https://pypi.org/project/proxystr/) lib. Also `Dream` takes a `Proxy` obj from that lib.
## **Advanced usage**
### Creating more than one task at once
- **sync**
```python
from wombo import Dream

dream = Dream()

tasks = []
for _ in range(3):
    tasks.append(dream.create_task('a dragon in the sky'))

# wait all of tasks
ready_tasks = dream.await_tasks(tasks)
for task in ready_tasks:
    task.image.save()

# or one by one as complited
for task in dream.as_complited(tasks):
    task.image.save()
```
- **async**
```python
import asyncio
from wombo import AsyncDream

dream = AsyncDream()

async def generate(amount: int):
    tasks = []
    for _ in range(amount):
        tasks.append(await dream.create_task('a dragon in the sky'))
    
    # wait all of tasks
    ready_tasks = await dream.await_tasks(tasks)
    for task in ready_tasks:
        task.image.save()
    
    # or one by one as complited
    async for task in dream.as_complited(tasks):
        task.image.save()

asyncio.run(generate(3))
```
### Context meneger
```python
from wombo import Dream

with Dream() as dream:
    task = dream.generate('a dragon in the sky')
    task.image.save()
```
- **async**
```python
import asyncio
from wombo import Dream

async def generate():
    async with Dream() as dream:
        task = await dream.generate('a dragon in the sky')
        task.image.save()
asyncio.run(generate())
```
## Support
Developed by `MrSmith06`: [telegram](https://t.me/Mr_Smith06) |  [gtihub](https://github.com/MrSmith06)
If you find this project helpful, feel free to leave a tip!
- EVM address (metamask): `0x6201d7364F01772F8FbDce67A9900d505950aB99`