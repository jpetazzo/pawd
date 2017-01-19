# pawd

PulseAudio Web Daemon

Goals:

- control volume of a PulseAudio daemon through a web interface
- allow to show existing "sink_inputs" and their metadata (if you're playing music, this often includes author and title)
- allow to move sink_inputs to different sinks (i.e. to send audio to different outputs)
- allow to manage combined sinks (=virtual outputs sending audio to multiple outputs simultaneously)

Useful doc: https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/Developer/Clients/WritingVolumeControlUIs/

## API

Everything in _italics_ is optional.

Each of the following routes will return an array containing the corresponding elements:

- /sinks
- /sink_inputs
- _/source_outputs_
- _/sources (optional)_

Each element is represented by a map with the following fields:

- index (unsigned integer)
- name (string)
- description (string)
- volume (unsigned integer)
- muted (boolean) (not present in source_output objects)
- _driver_ (string) 
- _volumes_ (array of integers corresponding to the volumes of all the channels)

For sinks and sources:
- `name` is the name reported by PulseAudio
- `description` is the `device.description` property

For sink_inputs:
- `name` is the `application.name` property
- `description` is the `media.name` property

(For source_outputs, I don't know, I haven't experimented with them yet!)

The following fields can be accessed directly with a `GET` request, and changed with a `PUT` request:
- description
- volume
- muted

The path should be `{object_type}/{index}/{field}`.

For instance, to set the volume of sink #3, one would `PUT` the volume (as an integer) to `/sink/3/volume`.
