<script>
/* eslint-disable */
/*

A Vue directive to check if the element is under or above the fold and add a class when the element is above.
<element v-scroll-trigger="{activeClass: 'is-active', offset: 30, relative: false }" ></element>

*/
export default {
	bind: function(el, binding) {
		// Get the absolute position of the element without translates

		let handle = {
			scroll: function() {
				const position = window.pageYOffset;
				if (position > settings.offset) {
					settings.scroll.top = false;
				} else {
					settings.scroll.top = true;
				}

				if (position <= _this.scroll.last) {
					settings.scroll.direction = 'up';
				} else {
					settings.scroll.direction = 'down';
				}

				handle.style();

				setTimeout(() => {
					settings.scroll.last = window.scrollY;
				});
			},
			style: function() {
				console.log('setting the style');
			}
		};

		// Set the default settings
		let settings = {
			debug: binding.value.debug ? binding.value.debug : false,
			opacity: binding.value.opacity ? binding.value.opacity : false,
			offClass: 'off',
			onClass: 'on',
			downClass: 'down',
			upClass: 'up',
			bemClass: null,
			offset: binding.value.offset ? binding.value.offset : 0,
			scroll: {
				direction: 'up',
				top: true,
				last: 0,
				ticking: false
			}
		};
		let init = {
			settings: function() {
				// Check if there are alternative settings.
				if (binding.value) {
					if (binding.value.bemClass) {
						if (binding.value.offClass) {
							settings.offClass = `${binding.value.bemClass}--${
								binding.value.offClass
							}`;
						}
						if (binding.value.onClass) {
							settings.onClass = `${binding.value.bemClass}--${
								binding.value.onClass
							}`;
						}
						if (binding.value.upClass) {
							settings.upClass = `${binding.value.bemClass}--${
								binding.value.upClass
							}`;
						}
						if (binding.value.downClass) {
							settings.downClass = `${binding.value.bemClass}--${
								binding.value.downClass
							}`;
						}
					} else {
						if (binding.value.offClass) {
							settings.offClass = binding.value.offClass;
						}
						if (binding.value.onClass) {
							settings.onClass = binding.value.onClass;
						}
						if (binding.value.upClass) {
							settings.upClass = binding.value.upClass;
						}
						if (binding.value.downClass) {
							settings.downClass = binding.value.downClass;
						}
					}
					// if (binding.value.offset) {
					// 	settings.offset = binding.value.offset;
					// }

					// if (binding.value.opacity) {
					// 	settings.offset = binding.value.offset;
					// }

					// if (binding.value.debug) {
					// 	settings.debug = binding.value.debug;
					// }
				}
			}
		};

		// Initialize the position.
		init.settings();

		// Add a event listener to a resizing window.
		window.addEventListener('scroll', function() {
			if (!this.scroll.ticking) {
				window.requestAnimationFrame(function() {
					handle.scroll();
					settings.scroll.ticking = false;
				});
				settings.scroll.ticking = true;
			}
		});
		handle.scroll();
	}
};
</script>
