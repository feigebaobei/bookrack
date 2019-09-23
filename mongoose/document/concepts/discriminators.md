# Discriminators

## model.discriminator()

为model的schema添加新的schema

```
var options = {discriminatorKey: 'kind'}
var eventScheme = new mongoose.Scheme({time: Date}, options)
var Event = mongoose.model('Event', eventSchema)
var ClickedLinkEvent = Event.discriminator('ClickedLinkEvent', new mongoose.Schema({url: String}, options))
// ClickedLinkEvent (它是一个model) 的schema就有url字段。
var clickedEvent = new ClickedLinkEvent({time: Date.now(), url: 'google.com'})
```

## 