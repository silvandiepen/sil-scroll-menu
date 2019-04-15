<script>
/* eslint-disable */
/*

A Vue directive to make (usually header menu) dissapear on scrolling down and appear on scrolling up. 
<element v-scroll-menu="{bemClass: 'header', setClasses: true, offset: 80 }" ></element>

*/
export default {
	bind: function(el, binding) {
		// Set the default settings
		let settings = {
			debug: binding.value.debug ? binding.value.debug : false,
			opacity: binding.value.opacity ? binding.value.opacity : false,
			setClasses: binding.value.setClasses ? binding.value.setClasses : false,
			setStyle: binding.value.setStyle ? binding.value.setStyle : true,
			offClass: binding.value.offClass ? binding.value.offClass : 'off',
			onClass: binding.value.onClass ? binding.value.onClass : 'on',
			downClass: binding.value.downClass ? binding.value.downClass : 'down',
			upClass: binding.value.upClass ? binding.value.upClass : 'up',
			bemClass: null,
			offset: binding.value.offset ? binding.value.offset : 0,
			scroll: {
				direction: 'up',
				top: true,
				last: 0,
				ticking: false
			}
		};

		let handle = {
			scroll: function() {
				const position = window.pageYOffset;
				if (position > settings.offset) {
					if (settings.scroll.top) {
						settings.scroll.top = false;
						handle.classes('top');
						handle.style('top');
					}
				} else {
					if (!settings.scroll.top) {
						settings.scroll.top = true;
						handle.classes('top');
						handle.style('top');
					}
				}

				if (position <= settings.scroll.last) {
					if (settings.scroll.direction !== 'up') {
						settings.scroll.direction = 'up';
						handle.classes('direction');
						handle.style('direction');
					}
				} else {
					if (settings.scroll.direction !== 'down') {
						settings.scroll.direction = 'down';
						handle.classes('direction');
						handle.style('direction');
					}
				}

				setTimeout(() => {
					settings.scroll.last = window.scrollY;
				});
			},
			style: function(set) {
				if (!settings.setStyle) {
					return;
				}
				switch (set) {
					case 'top':
						break;
					case 'direction':
						if (settings.scroll.direction == 'up') {
							if (settings.opacity) {
								el.style.opacity = 1;
							}
							el.style.transform = 'translateY(0%)';
						} else {
							if (settings.opacity) {
								el.style.opacity = 0;
							}
							el.style.transform = 'translateY(-100%)';
						}
						break;
				}
			},
			classes: function(set) {
				if (!settings.setClasses) {
					return;
				}
				switch (set) {
					case 'top':
						if (settings.scroll.top) {
							el.classList.remove(settings.offClass);
							el.classList.add(settings.onClass);
						} else {
							el.classList.remove(settings.onClass);
							el.classList.add(settings.offClass);
						}
						break;
					case 'direction':
						if (settings.scroll.direction == 'up') {
							el.classList.remove(settings.downClass);
							el.classList.add(settings.upClass);
						} else {
							el.classList.remove(settings.upClass);
							el.classList.add(settings.downClass);
						}
						break;
				}
			}
		};

		let init = {
			settings: function() {
				// Check if there are alternative settings.
				if (binding.value) {
					if (binding.value.bemClass) {
						let bem = binding.value.bemClass;
						settings.offClass = `${bem}--${settings.offClass}`;
						settings.onClass = `${bem}--${settings.onClass}`;
						settings.upClass = `${bem}--${settings.upClass}`;
						settings.downClass = `${bem}--${settings.downClass}`;
					}
				}
				console.log(settings);
			}
		};

		// Initialize the position.
		init.settings();

		// console.log(settings);
		
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
