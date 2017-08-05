Home of clients for the lapiz clients

- [python](python)
- [lua](lua)

See the server-side [README](https://github.com/bosr/lapiz) for an overview and more information on the JSON specs.

Note: Lapiz is a temporary fork of [Crayon](https://github.com/torrvision/crayon).


## Example usage with the Python library

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

More details in [python/README.md](python/README.md).
