# Style Guide for KERIpy, HIO, and related development

This style guide shows agreed upon recommendations for writing both Python code for KERIpy, HIO, KERIA, and SignifyPy and Typescript code in SignifyTS. Included in this stile guide are things like proper use of Hio's Doers, naming standards, Python module conventions, and more.

## Contents

1. Naming Standards
2. Idiomatic HIO Usage for Doers
3. Use of Cues for intercomponent communication


## Naming Standards

`camelCase` for class names, function names, and all names unless required otherwise for library compatibility.

See the [KERIpy naming standards](https://github.com/WebOfTrust/keripy/blob/main/ref/naming.md) for more.

## HIO Usage

### doing.doify vs Doer with .recur()

#### When a sub generator (using yield or yield from) is not needed

doing.doify example for when the inner recur does not need to yield.

The following example uses doing.doify and has a yield yet it does not need one because it does not run the constructed MigrateDoer coroutines and instead hands them to the MigrateDoDoer to run them.

```python
class MigrateDoDoer(doing.DoDoer):
    def __init__(self, args):
        self.args = args
        self.adb = basing.AgencyBaser(name="TheAgency", base=args.base, reopen=True, temp=False)
        doers = [doing.doify(self.migrateAgentsDo)]
        super(MigrateDoDoer, self).__init__(doers=doers)

    def migrateAgentsDo(self, tymth, tock=0.0):
        """ For each agent in the agency, add a migrate doer

        Parameters:
            tymth (function): injected function wrapper closure returned by .tymen() of
                Tymist instance. Calling tymth() returns associated Tymist .tyme.
            tock (float): injected initial tock value

        """
        self.wind(tymth)
        self.tock = tock
        _ = (yield self.tock)

        for ((caid,), _) in self.adb.agnt.getItemIter():
            args = run.parser.parse_args(['--name', caid, '--base', self.args.base, '--temp', False])
            self.extend([run.MigrateDoer(args)])
        return True        
```

The preceding example can be simplified to have a `.recur()` call in the MigrateDoDoer that calls self.migrateAgentsDo().
If the `.recur()` function is called in a DoDoer then the superclass `.recur()` function must also be called to resume the normal DoDoer execution of all its contained coroutines (Doers).

```python
class MigrateDoDoer(doing.DoDoer):
    def __init__(self, args):
        self.args = args
        self.adb = basing.AgencyBaser(name="TheAgency", base=args.base, reopen=True, temp=False)
        doers = [doing.doify(self.migrateAgentsDo)]
        super(MigrateDoDoer, self).__init__(doers=doers)

    def migrateAgentsDo(self):
        """For each agent in the agency, add a migrate doer."""
        for ((caid,), _) in self.adb.agnt.getItemIter():
            args = run.parser.parse_args(['--name', caid, '--base', self.args.base, '--temp', False])
            self.extend([run.MigrateDoer(args)])
        
    def recur(self, tyme, deeds=None):
        self.migrateAgentsDo()
        super(MigrateDoDoer, self).recur(tyme, deeds)
        return True
```
