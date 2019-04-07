# rocketgram

Modern and powerful asynchronous telegram bot framework.

# Minimal requirements

* Python 3.7
* aiohttp 3.5.4

# Example

There is a trivial example.

```python
from rocketgram import Bot, Context, Dispatcher, commonfilters, run_updates

token = 'YOUR_BOT_TOKEN'

router = Dispatcher()
bot = Bot(token, router=router)

@router.handler
@commonfilters.command('/start')
async def start_command(ctx: Context):
    await ctx.send_message('Hello there!')
    
@router.handler
@commonfilters.command('/help')
async def start_command(ctx: Context):
    await ctx.send_message('Some userful help!')
    
run_updates(bot)
```

# Making your waiters
```python
@make.waiter
@commonfilters.update_type(UpdateType.message) #Specify update type
@commonfilters.chat_type(ChatType.private) #ChatType contains private, group, supergoup and channel
def next_all(ctx: Context): -> Returns bool
    return True
  ```
# Using your waiters
```python
@router.handler
@commonfilters.chat_type(ChatType.private)
@commonfilters.command('/newbot')
async def create_new_bot(ctx: Context):
    await ctx.send_message("Alright, a new bot. How are we going to call it? Please choose a name for your bot.")
    while True:
        ctx: Context = (yield next_all())

        if ctx.update.message.text == '/cancel':
            await ctx.send_message("The command newbot has been cancelled. Anything else I can do for you?")
            return
            ```
