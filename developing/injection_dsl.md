# Wirebox Injection DSL

Everything inside CommandBox is built via Wirebox. This means your models, commands, and interceptors can all take advantage of Wirebox's dependency injection.   All you need to do is add a property to the top of your CFC that tells WireBox what to inject.

## Core Models

There are a number of automatically-mapped objects inside WireBox.  Here are some examples:

```javascript
property name='shell' inject='commandbox';
property name='tempDir' inject='tempDir@constants';
property name='formatterUtil' inject='formatter';
property name='ModuleService' inject='ModuleService';
property name='ConfigService' inject='ConfigService';

```

## Custom Models

## Module Config and Settings

## CommandBox Config 