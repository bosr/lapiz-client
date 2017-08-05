Lapiz client is a python library for sending summaries to Tensorboard over HTTP.

# Example usage
More doc is on the way.

In your project virtualenv, install the [lapiz-client](https://github.com/bosr/lapiz-client) python library:

    pip install lapiz-client  # available on Pypi

From a client:

    import lapiz as lz

    cl = lz.Client('192.168.78.36', port=6007)  # check connection to lapiz server (6007 by default)

    with cl.run('run0,lr=1e-3,lambda=1e-1') as run:
        for _ in range(num_steps):

            # ...

            # the global_step starts at 0 by default, but override it with run.step(1)
            run.step()  # now step is 1

            run.add_scalar('accuracy', 1.3e-3)            # numpy types accepted
            run.add_histogram('hidden_weights', weights)  # flatten your weights
            run.add_image('sample', img)
            run.add_audio('audio', waveform)

            # run.save()  # not needed unless you cannot wait for the next run.step()

Each time the `run.step()` instruction is executed, the client saves all
unsaved summaries, if any. You can save those summaries as early as you want
with `run.save()`. The context manager makes it sure unsaved summaries will be
saved at exit.

If the context manager style adopted by `run` does not fit you, use it directly
but just don't forget to save and close:

    run = cl.run('run0,lr=1e-3,lambda=1e-1')
    run.step(36)
    run.add_scalar('accuracy', 1e-3)
    run.save()  # send to lapiz server
    run.step(37)
    run.add_scalar('G_loss', 1e-1)
    run.wall_time(11.3)  # arbitrary wall_time
    run.save()
    run.close()  # or del run

The wall time associated to events is taken automatically when `run.save()` is
executed, but it can be overwritten with `run.wall_time(time.time())`.
