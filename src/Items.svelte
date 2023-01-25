<script context="module">
    export const categories = ['', 'Pizza', 'Sides'];
    export const varients = ['', 'Chips', 'Onion Rings'];
    export const sizes = ['', 'Large', 'Small'];
    export const pizzaSizes = ['', '18-inch', '12-inch', '10-inch'];
    
    export const orderStatus = ['', 'Ordered', 'In the kitchen', 'Ready for collection', 'Collected'];
    
    export function parseItem(item) {
        let string = '';
        
        if (item.category == 1) {
            string += categories[item.category] + ', ';
        }
        
        if (typeof(item.variant) !== "undefined") {
            string += varients[item.variant] + ', ';
        }
        
        if (item.category == 1) {
            string += pizzaSizes[item.size];
        } else {
            string += sizes[item.size];
        }
        
        return string
    }
</script>