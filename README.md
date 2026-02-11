# lustre_event_source

Lustre bindings for
[`EventSource`](https://developer.mozilla.org/en-US/docs/Web/API/EventSource).

<!--
[![Package Version](https://img.shields.io/hexpm/v/lustre_event_source)](https://hex.pm/packages/lustre_event_source)
[![Hex Docs](https://img.shields.io/badge/hex-docs-ffaff3)](https://hexdocs.pm/lustre_event_source/)

```sh
gleam add lustre_event_source@0.2.0
```
```gleam
import lustre_event_source

pub fn main() -> Nil {
  // TODO: An example of the project in use
}
```

Further documentation can be found at <https://hexdocs.pm/lustre_event_source>.-->

## Example

```gleam
import lustre_event_source
import lustre/effect

type Model {
  Model(event_source: option.Option(lustre_event_source.EventSource))
}

type Msg {
  EventSource(lustre_event_source.Message)
}

fn init(_) {
  #(
    Model(event_source: option.None),
    lustre_event_source.init("/path/to/event/source", EventSource),
  )
}

fn view(model) {
  // Your view code
  todo
}

fn update(model, msg) {
  case msg {
    EventSource(lustre_event_source.Data(data)) -> {
      // Do something with your data
      echo data
      #(model, effect.none())
    }
    EventSource(lustre_event_source.Init(event_source))
    | EventSource(lustre_event_source.OnOpen(event_source)) -> {
      // WARNING: if you ever use `lustre_event_source.ready_state()` in your
      //          view function, then you need to ensure that your model is
      //          *changed* here. Otherwise, lustre won't update your view.
      #(
        Model(event_source: option.Some(event_source)),
        effect.none(),
      )
    }
    EventSource(lustre_event_source.Error) -> {
      // Uh oh there was an error.
      // Handle it. Or don't.
      // WARNING: if you ever use `lustre_event_source.ready_state()` in your
      //          view function, then you need to ensure that your model is
      //          *changed* here. Otherwise, lustre won't update your view.
      #(model, effect.none())
    }
    EventSource(lustre_event_source.NoEventSourceClient) -> #(
      Model(event_source: option.None),
      effect.none(),
    )
  }
}
```
