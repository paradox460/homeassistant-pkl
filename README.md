# Homeassistant Pkl types

This package contains [Pkl](https://pkl-lang.org) types for Home Assistant YAML configuration files.

They are automatically generated from the [HomeAssistant VSCode plugin](https://github.com/keesschollaart81/vscode-home-assistant).

## Usage

In a Pkl file, you can use them like this:

```pkl
import "package://pkg.pkl-lang.org/github.com/paradox460/homeassistant-pkl@2025.7.21#/type/IntegrationTemplate.pkl"

binary_sensor = new Listing {
  new {
    name = "Door Left Open"
    unique_id = name.sha1
    icon = "mdi:door-open"
    state = """
    {{ label_entities("door sensor") | map("states") |  select("equalto", "open") | first == "open" }}
    """
  }
  new {
    name = "Doors Closed"
    unique_id = name.sha1
    icon = "mdi:door"
    state = """
    {{ label_entities("door sensor") | map("states") |  select("equalto", "closed") | first == "closed" }}
    """
  }
}

output {
  renderer = new YamlRenderer {}
}
```

If you run that pkl with `pkl eval`, you will get the following yaml output:

```yaml
binary_sensor:
- name: Door Left Open
  unique_id: 88f60ebfff0f2811b3a0a3c84355704a78fc3d00
  icon: mdi:door-open
  state: '{{ label_entities("door sensor") | map("states") |  select("equalto", "open") | first == "open" }}'
- name: Doors Closed
  unique_id: 4da710019ce071440e7752b22f2888d011983961
  icon: mdi:door
  state: '{{ label_entities("door sensor") | map("states") |  select("equalto", "closed") | first == "closed" }}'
```

## Rationale

Writing YAML isnt' really pleasant. Writing Pkl is pleasant. Thats my main motivation for this package.

If thats not a strong enough motivation for you, consider that you can make reusable objects (i.e. a binary sensor that only varies slightly for many different base entities) and iterate over them very quickly, generating a syntactically correct YAML without all the indentation tracking headaches.
