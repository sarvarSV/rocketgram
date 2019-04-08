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

# A little about update_type
```python
#update_type is field 
#what is calculated on the basis of incoming data.


@router.handler
@commonfilters.chat_type(ChatType.private)
@commonfilters.update_type(UpdateType.message) -> #There we used update_type handler to catch only messages from users
async def return_all_messages(ctx: Context):
       await ctx.send_message(ctx.update.message.text, reply_markup=kb.kb.render())
        if ctx.update.message.text == '/cancel':
            await ctx.send_message("Cancelled")
            return
        if ctx.update.message.text: -> #There we can get a text from user
            await ctx.send_message(ctx.update.message.text, reply_markup=kb.kb.render())
```
# Inline keyboards
```python
#Unlike other libraries, rocketgram using a bit-different form of inline keyboards.

from rocketgram import InlineKeyboardMarkup, InlineKeyboardButton, InlineKeyboard

kb = InlineKeyboard() -> it's a class that renders keyboard types conveniently
kb.callback('First kb', 'kb1').row().callback('Second kb', 'kb2').row().callback('Third kb', 'kb3') -> #row() method used
#to insert a "break"

```
# Using inline keyboards
```python
#Inline buttons that we made above very easy to use

@router.handler
@commonfilters.chat_type(ChatType.private)
@commonfilters.update_type(UpdateType.message)
async def return_all_messages(ctx: Context):
       await ctx.send_message(ctx.update.message.text, reply_markup=kb.kb.render()) -> #Render used to generate markup of the #appropriate type
```
