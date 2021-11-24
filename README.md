# variant-inventory-status


          product.liquid Html
          ========
          
          {% assign vnormal = false %}{% assign vcontinue = false %}{% assign voutofstock = false %}
            {% for vi in product.tags %}
            {% if vi contains 'Normal:' %}{% assign vnormal = true %}{% assign vnormalvalue = vi | remove: 'Normal:' %}{% endif %}
            {% if vi contains 'Continue:' %}{% assign vcontinue = true %}{% assign vcontinuevalue = vi | remove: 'Continue:' %}{% endif %}
            {% if vi contains 'Outofstock:' %}{% assign voutofstock = true %}{% assign voutofstockvalue = vi | remove: 'Outofstock:' %}{% endif %}
            {% endfor %}
            <div id="variant-inventory" class="variant-inventory">
              <span class="variant_status variant_status_normal{% if variant.inventory_quantity >= 1 and variant.inventory_policy != 'continue' %} show{% else %} hide{% endif %}">{% if vnormal == true %}{{vnormalvalue}}{% else %}Forventet levering: 1 - 3 hverdage{% endif %}</span>
              <span class="variant_status variant_status_continue{% if variant.inventory_quantity <= 0 and variant.inventory_policy == 'continue' %} show{% else %} hide{% endif %}">{% if vcontinue == true %}{{vcontinuevalue}}{% else %}Ikke på lager - Forventet levering: Op til 12 hverdage{% endif %}</span>
              <span class="variant_status variant_status_outofstock{% if variant.inventory_quantity <= 0 and variant.inventory_policy != 'continue' %} show{% else %} hide{% endif %}">{% if voutofstock == true %}{{voutofstockvalue}}{% else %}Udsolgt{% endif %}</span>
            </div>
            
            product.liquid js
            ===========

             <script>
              window._VARIANTSINV = {};
              {% for varianti in product.variants %}
              window._VARIANTSINV[{{ varianti.id }}] = {{ varianti.inventory_quantity }}
              {% endfor %}
            </script>
            <script>
              window._VARIANTSPOL = {};
              {% for variantp in product.variants %}
              window._VARIANTSPOL[{{ variantp.id }}] = '{{ variantp.inventory_policy }}'
              {% endfor %}
            </script>

            theme.js
            ============
            
            this.selectors = {
                $inventoryContainer: $('.variant-inventory', this.$container)
            }
	  
	  
                ------------------------------------


          var vinventory = _VARIANTSINV[variant.id];
              var vpolicy = _VARIANTSPOL[variant.id];
              if (vinventory >= 1 && vpolicy != 'continue') { console.log('normal');
                this.selectors.$inventoryContainer.find('.variant_status').removeClass('show').addClass('hide');
                this.selectors.$inventoryContainer.find('.variant_status_normal').removeClass('hide').addClass('show');
              } else if (vinventory <= 0 && vpolicy == 'continue') { console.log('continue');
                this.selectors.$inventoryContainer.find('.variant_status').removeClass('show').addClass('hide');
                this.selectors.$inventoryContainer.find('.variant_status_continue').removeClass('hide').addClass('show');
              } else if (vinventory <= 0 && vpolicy != 'continue') { console.log('outofstock');
                this.selectors.$inventoryContainer.find('.variant_status').removeClass('show').addClass('hide');
                this.selectors.$inventoryContainer.find('.variant_status_outofstock').removeClass('hide').addClass('show');
              }
